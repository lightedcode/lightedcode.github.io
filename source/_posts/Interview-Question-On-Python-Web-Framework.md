---
title: Interview Questions On Python Web Framework
date: 2024-08-04 22:34:42
cover_image:
cover_image_alt:
tags:
    - Interview
    - Web Framework
categories:
    - Code
---

# WSGI（Python Web Server Gateway Interface）
[PEP 3333 – Python Web Server Gateway Interface v1.0.1](https://peps.python.org/pep-3333/)
- 描述了Web Server（gunicorn/uWSGI）如何与web框架交互（Flask， Django），web框架如何处理请求

```python
def application(environ,  start_response):
 """
参数
- environ: 一个包含WSGI环境信息的字典，由WSGI服务器提供，常见的key有PATH_INFO,QUERY_STRING等

- start_response：生成WSGI响应的回调函数，接受两个参数：status和headers

函数返回响应体的可迭代对象
"""
```

# Django vs Flask vs Tornado

- Django：大而全，内置ORM，Admin等组建，第三方较多
- Flask微框架：插件机制，比较灵活
- Tornado：异步支持的微框架和异步网络库

# MVC（Model View Controller）

解耦数据展示和操作

- Model：负责业务对象和数据库的交互（ORM）
- View：负责与用户的交互展示
- Controller：接收参数调用模型和视图完成操作

# ORM（Object Relational Mapping）对象关系映射

- 用于实现业务对象与数据表中的字段映射
- 优势：代码更加面向对象，代码量更少，灵活性高，提升开发效率

# Web安全
## SQL注入

- 通过构造特殊的输入参数传入Web应用，导致后端执行了恶意SQL
- 通常由于程序员未对输入进行过滤，直接动态拼接SQL产生
- 可使用开源工具sqlmap， SQLninja检测 

### 如何防范SQL注入：

- 对输入参数做好检查（类型和范围）；过滤和转义特殊字符
- 不要直接拼接sql， 使用ORM可以大大降低sql注入的风险
- 数据库层：做好权限管理配置，不要明文存储敏感信息

## XSS（跨站脚本攻击，Cross-Site Scriping）
- 恶意用户将代码植入到给其他用户使用的页面中，未经转义的恶意代码输入到其他用户的浏览器被执行
- 用户浏览页面的时候嵌入页面的js脚本被执行，攻击用户
- 主要分为两类：反射性（非持久型），存储型（持久型）

### 如何防范XSS攻击：

- 对用户输入进行严格的过滤和转义，特别是HTML、JavaScript、CSS等。
- 使用安全的API，避免直接将用户输入嵌入到HTML中。
- 使用内容安全策略（Content Security Policy, CSP）来限制哪些资源可以被加载和执行。

## CSRF（跨站请求伪造，Cross-Site Request Forgery）

- 恶意网站诱导用户浏览，从而在用户不知情的情况下，向受信任的网站发送伪造的请求。
- 攻击者利用用户的身份认证信息（如Cookie），在用户不知情的情况下执行操作。

### 如何防范CSRF攻击：

- 使用CSRF令牌（Token）：在每个请求中包含一个随机生成的令牌，服务器验证令牌的有效性。
- 检查Referer头：验证请求的来源是否可信。
- 使用SameSite属性：在Cookie中设置SameSite属性，限制Cookie的跨站发送。

# 前后端分离和RESTful
- 前后端解耦，接口复用，减少开发量
- 各司其职，前后端同步开发，提升工作效率，定义好接口规范
- 有利于调试（mock），测试和运维部署

## RESTful（Representational State Transfer）

- 表现层状态转移，由HTTP协议的主要设计者Roy Fielding提出
- 资源（Resources）：使用URI指向的一个实体
- 表现层（Representation）：资源的表现形式，比如图片，HTML文本等
- 状态转化（State Transfer）：GET，POST，PUT，DELETE等HTTP动词来操作资源，实现资源状态的改变
- 是一种以资源为中心的web软件架构风格，可以用Ajax和RESTfulweb服务构建应用

### RESTful准则

- 所有事物抽象为资源（resource），资源对应唯一的标识（id）
- 资源通过接口来实现状态转移，操作本身是无状态的
- 对资源的操作不会改变资源的标识

### RESTful API

- 通过 HTTP GET/POST/PUT/DELETE
- 一般使用json格式返回数据
- 一般web框架都有相应的插件支持RESTful

# 什么是HTTPS

## 什么是对称加密和非对称加密

### 对称加密

对称加密是加密解密双方都使用同一个密钥进行加解密，优点是速度快，缺点是泄露密钥后容易泄密。常见的对称加密有：DES AES等

### 非对称加密

非对称加密是加解密双方都有一对密钥，一个是公钥，一个是私钥，数据可以使用公钥加密，然后使用私钥解密。加密方获得了解密方的公钥并使用该公钥对数据加密发送给解密方，解密方使用自己的私钥进行解密。优点是安全性高，缺点是速度慢。常见的非对称加密有：RSA DSA

## HTTPS的通信过程是什么样的

### 1. HTTPS 和 HTTP 的区别

HTTP（HyperText Transfer Protocol）和 HTTPS（HyperText Transfer Protocol Secure）都是用于在客户端（如浏览器）和服务器之间传输数据的协议。它们的主要区别如下：

- **安全性**：
  - **HTTP**：数据以明文形式传输，容易被窃听和篡改。
  - **HTTPS**：在 HTTP 的基础上加入了 SSL/TLS 协议，对数据进行加密传输，确保数据的机密性、完整性和真实性。
  
- **端口**：
  - **HTTP**：默认使用端口 80。
  - **HTTPS**：默认使用端口 443。

- **证书**：
  - **HTTP**：不需要证书。
  - **HTTPS**：需要 SSL/TLS 证书，由受信任的证书颁发机构（CA）签发。

- **性能**：
  - **HTTP**：由于没有加密和解密的开销，性能稍好。
  - **HTTPS**：由于加密和解密的操作，性能略有下降，但现代硬件和优化技术已经使这个影响很小。

### 2. HTTPS 的通信过程

HTTPS 的通信过程可以分为以下几个主要步骤：

1. **客户端发起请求**：
   - 客户端（如浏览器）向服务器发起 HTTPS 请求，要求建立安全连接。

2. **服务器响应并发送证书**：
   - 服务器接收到请求后，发送一个包含公钥的 SSL/TLS 证书给客户端。这个证书是由受信任的证书颁发机构（CA）签发的。

3. **客户端验证证书**：
   - 客户端接收到服务器的证书后，会验证证书的有效性，包括检查证书是否在有效期内，是否由受信任的 CA 签发，是否与服务器域名匹配等。

4. **生成会话密钥**：
   - 如果证书验证通过，客户端会生成一个随机的会话密钥（对称密钥），并使用服务器的公钥对该会话密钥进行加密，然后发送给服务器。

5. **服务器解密会话密钥**：
   - 服务器使用自己的私钥解密客户端发送的会话密钥，从而获得这个对称密钥。

6. **加密通信**：
   - 此时，客户端和服务器都拥有相同的会话密钥，后续的通信数据将使用这个对称密钥进行加密和解密，确保数据传输的安全性。

7. **结束连接**：
   - 当通信结束时，客户端和服务器会协商关闭 SSL/TLS 连接。