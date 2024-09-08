---
title: Interview Questions On System Design
date: 2024-08-05 10:30:40
cover_image:
cover_image_alt:
tags:
    - Interview
    - System Design
categories:
    - Code
---

# 什么是系统设计

- 系统设计是一个定义系统架构，模块，接口和数据满足特定需求的过程
- 比如设计一个短网址服务，评论服务，Feed流系统，抢红包服务
- 微服务架构很多系统被按照业务拆分，需要单独设计一个系统服务

# 系统设计需要掌握哪些知识

- 需要具备相关领域，算法的经验，有一定的架构设计能力
- 熟悉后端技术组件，比如消息队列，缓存，数据库，框架
- 具备文档撰写，流程图绘制，架构设计，编码实现等综合能力 

# 系统设计三大要素：如何设计和实现一个后端系统服务的设计

## 1. 使用场景和限制条件

1. 问面试官：什么场景和条件下使用
- 这个系统实在什么地方使用的？比如点网址系统提供给站内各种服务生成短网址
- 限制条件：用户估计有多少？至少要支撑多少用户？
- 估算并发qps：峰值qps是多少？平均qps是多少

## 2. 数据存储设计

- 按需设计数据表：需要哪些字段，使用什么类型？数据增长规模
- 数据库选型：是否需要持久化？使用关系性还是NoSQL？
- 如何优化？如何设计索引？是否可以使用缓存？

## 3. 算法模块设计

算法是解决问题的核心。程序=算法+数据结构。系统=服务+存储

- 需要哪些接口？接口如何设计？
- 使用什么算法或者模型
- 不同实现方式之间的优劣对比？如何取舍？

## 4. 延伸考点（如何扩展？容错？）

- 用户多了，qbs高了如何处理？
- 数据存储多了不够存了如何处理？
- 故障如何处理？单点失败，多点失败，雪崩问题

# 短网址系统设计与实现

## 1. 什么是短网址系统？包含哪些功能（接口）

- 把长网址缩短为短网址的服务，比如[bitly.com](bitly.com)，转换之后的网址后缀不超过7位（字符或者数字）
- 功能：一个长网址转换成短网址并存储，根据短网址还原长url
- 要求短网址的后缀不超过7为（大小写字母和数字）
- 预估峰值插入请求数量级（数百），查询请求数量级（数千）

## 2. 存储设计？需要存储哪些字段

- MySQL就可以
- 需要哪些字段：
  - id，
  - token(**索引**)
  - url(原网址)
  - created_at
  
## 3. 如何设计算法生成短网址？

### API接口

1. long2short
2.  short2long


### 使用什么算法？

1. 哈希函数截取：md5，取前七个。但是可能会有冲突


2. 自增序列算法
使用charset = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'
一共62个字符

可以通过一个自增的id把10进制转换为62进制的

```python
charset = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'

 def encode(num):
    if num == 0:
        return charset[0]
    res = []
    while num:
        num, rem = divmod(num, len(charset))
        res.append(charset[rem])
    return "".join(reversed(res))

```


# 如何设计一个秒杀系统

## 1. 使用场景和限制条件

## 2. 数据库表的设计

## 3. 算法和接口

### 设计原理

令牌桶算法（Token Bucket Algorithm）是通过控制请求的速率来限制流量的。其基本原理如下：

1. **令牌桶**：系统维护一个令牌桶，桶内可以容纳一定数量的令牌。
2. **生成令牌**：令牌按照固定的速率生成，并放入桶中。如果桶满了，新生成的令牌会被丢弃。
3. **请求令牌**：每个请求到达时，需要从桶中取出一个令牌。如果桶中有令牌，则请求被允许通过；如果没有令牌，则请求被拒绝或排队等待。

### 实现步骤

1. **初始化令牌桶**：
   - 设置桶的容量 \( C \) 和令牌生成速率 \( R \)（每秒生成的令牌数）。
   - 初始化桶内的令牌数为 0。

2. **生成令牌**：
   - 定时任务（如每秒执行一次）向桶中添加令牌，添加数量为 \( R \)。
   - 如果桶中的令牌数达到或超过容量 \( C \)，则保持为 \( C \)。

3. **请求处理**：
   - 当请求到达时，检查桶中是否有令牌。
   - 如果有令牌，则从桶中取出一个令牌，允许请求通过。
   - 如果没有令牌，则拒绝请求或将请求放入等待队列。

### 示例代码

```python
import time
import threading

class TokenBucket:
    def __init__(self, capacity, rate):
        self.tokens = capacity
        self.capacity = capacity
        self.rate = rate
        self.last_generate_time = time.time()
        self.lock = threading.Lock()
    def fill_bucket(self):
        now = time.time()
        self.tokens = min(self.capacity, self.tokens + (now - self.last_generate_time) * self.rate)
        self.last_generate_time = now
    def get_token(self):
        with self.lock:
            self.fill_bucket()
            if self.tokens > 0:
                print("获取到token")
                self.tokens -= 1
            else:
                print("未获取到token")

if __name__ == '__main__':
    tb = TokenBucket(10, 3)
    
    for i in range(20):
        tb.get_token()
        time.sleep(0.1)

```





