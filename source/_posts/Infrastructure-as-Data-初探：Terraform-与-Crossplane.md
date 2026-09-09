---
title: Infrastructure as Data 初探：Terraform 与 Crossplane
date: 2026-09-09 17:31:23
cover_image:
cover_image_alt:
thumbnail:
tags:
    - Terraform
    - Crossplane
    - IaC
categories:
    - Code
---

# 1. 从 Infrastructure as Code 到 Infrastructure as Data

过去十年，我们习惯了用「代码」来描述基础设施——Infrastructure as Code（IaC）。
你写一段声明式配置，工具帮你把云上的资源拉成你描述的样子。但当团队和资源规模变大后，
一个更进一步的理念开始流行：**Infrastructure as Data（IaD，基础设施即数据）**。

两者的区别，一句话概括：

- **IaC**：基础设施的期望状态写在「配置文件/代码」里，由一个外部工具（如 Terraform）读取并执行。
- **IaD**：基础设施的期望状态本身就是一份「数据」，存在于一个持续运行的控制平面里，
  由控制器不断地把现实「调谐（reconcile）」到这份数据描述的状态。

关键差异在于「谁来持续保证状态」。IaC 通常是「你跑一次命令，它对齐一次」；
IaD 则是「一个控制器永远盯着，一旦漂移就自动拉回」。

# 2. Terraform：IaC 的代表

Terraform 是目前最主流的 IaC 工具，用自己的声明式语言 HCL 描述资源：

```hcl
resource "aws_s3_bucket" "blog" {
  bucket = "lightedcode-blog-assets"
  tags = {
    Project = "blog"
  }
}
```

它的工作模型很直观：

1. 你写 `.tf` 文件，描述期望状态。
2. `terraform plan` 对比「期望状态」和「当前状态」（记录在 state 文件里），算出差异。
3. `terraform apply` 执行差异，把云资源变成你要的样子。

**优点**：生态成熟、provider 覆盖几乎所有云、心智模型简单。

**痛点**：

- **state 文件**是核心也是负担——它记录了 Terraform 眼中的世界，一旦和现实不一致（比如有人手动改了云控制台），就会漂移。
- 它是「一次性」的：你不 `apply`，它就不会主动纠正漂移。
- 大团队协作时，state 锁、模块拆分、CI 编排都会变复杂。

# 3. Crossplane：把基础设施变成 Kubernetes 里的数据

Crossplane 换了个思路：**把云资源变成 Kubernetes 的自定义资源（CRD）**，
让 Kubernetes 的控制器循环来持续管理它们。

同样是建一个 S3 桶，在 Crossplane 里长这样：

```yaml
apiVersion: s3.aws.crossplane.io/v1beta1
kind: Bucket
metadata:
  name: lightedcode-blog-assets
spec:
  forProvider:
    region: ap-east-1
  providerConfigRef:
    name: aws-provider
```

你把这段 YAML `kubectl apply` 到集群里，它就成了集群里的一条「数据」。
之后 Crossplane 的控制器会**持续地**保证这个桶存在、且配置符合描述——
这就是 IaD 的精髓：**期望状态是集群里的数据，调谐是持续进行的**。

这带来几个 Terraform 不具备的特性：

- **持续调谐**：有人手动删了桶，控制器会自动把它建回来，无需你重新 `apply`。
- **无独立 state 文件**：期望状态和实际状态都由 Kubernetes API + 控制器管理，不再有单独的 state 要维护。
- **可组合与抽象**：通过 Composition，平台团队可以把「一个数据库 + 一个桶 + 一套网络」打包成一个高层抽象（XRD），
  暴露给业务团队一个简单接口，隐藏底层细节。

# 4. 该怎么选

它们并不是非此即彼，很多团队会混用。给一个粗略的判断：

| 维度 | Terraform | Crossplane |
|---|---|---|
| 心智模型 | 命令行「一次性对齐」 | 控制器「持续调谐」 |
| 状态管理 | 独立 state 文件 | Kubernetes API，无独立 state |
| 漂移处理 | 需重新 apply | 自动纠正 |
| 前置成本 | 低，装个 CLI 即可 | 高，需要一个 Kubernetes 集群 |
| 适合场景 | 中小团队、一次性供给、混合环境 | 平台工程、大规模自服务、已重度使用 K8s |

- 如果你只是想快速、可复现地供给一批云资源，又没有现成的 Kubernetes 平台，**Terraform 依然是最省事的选择**。
- 如果你在做**平台工程（Platform Engineering）**，想给业务团队提供「自助申请基础设施」的能力，
  且已经全面拥抱 Kubernetes，**Crossplane 的 IaD 模型会更契合**。

# 5. 小结

Infrastructure as Data 不是要取代 Infrastructure as Code，而是把「声明式」推得更彻底：
从「声明一份配置，然后跑工具去对齐」，进化到「声明的数据本身就活在一个持续调谐的控制平面里」。

Terraform 教会我们用代码描述基础设施；Crossplane 则让这份描述变成系统里持续生效的数据。
理解这层区别，比记住某个工具的语法更重要。
