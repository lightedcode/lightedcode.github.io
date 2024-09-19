---
title: Mastering Design Patterns in Python
date: 2024-08-01 17:51:21
cover_image: /image/cover003.png
cover_image_alt: AI LOGO
thumbnail: /image/cover003.png
tags:
    - Design Patterns
    - Algorithm
    - Interview
categories:
    - Code
---
# 简介
在软件开发过程中，设计模式是一种经过验证的、可重复使用的解决方案，用于解决常见的设计问题。无论是初学者还是有经验的开发者，理解和应用设计模式都能帮助我们编写更健壮、可维护和可扩展的代码。
# 创建型模式
## 工厂模式（Factory）：解决对象创建问题
- 工厂方法和抽象工厂
- 解耦对象的创建和使用

```python
class DogToy:
    def speak(self):
        print("wang wang")


class CatToy:
    def speak(self):
        print("miao miao")


def toy_factory(toy_type):
    if toy_type == 'dog':
        return DogToy()
    elif toy_type == 'cat':
        return CatToy()

```

## 构造模式（Builder）：控制复杂对象的创建
- 控制复杂对象的构造
- 创建和表示分离。比如买电脑：工厂模式直接给你需要的电脑
- 构造模式允许自己定义电脑的配置，组装完成后再给你


```python
# 定义电脑类
class Computer:
    def __init__(self):
        self.cpu = None
        self.ram = None
        self.storage = None

    def __str__(self):
        return f"Computer with CPU: {self.cpu}, RAM: {self.ram}GB, Storage: {self.storage}GB"

# 定义构建者接口
class ComputerBuilder:
    def build_cpu(self):
        pass

    def build_ram(self):
        pass

    def build_storage(self):
        pass

    def get_computer(self):
        pass

# 具体的构建者类
class SimpleComputerBuilder(ComputerBuilder):
    def __init__(self):
        self.computer = Computer()

    def build_cpu(self):
        self.computer.cpu = "Basic CPU"

    def build_ram(self):
        self.computer.ram = 4

    def build_storage(self):
        self.computer.storage = 128

    def get_computer(self):
        return self.computer

class AdvancedComputerBuilder(ComputerBuilder):
    def __init__(self):
        self.computer = Computer()

    def build_cpu(self):
        self.computer.cpu = "Advanced CPU"

    def build_ram(self):
        self.computer.ram = 16

    def build_storage(self):
        self.computer.storage = 512

    def get_computer(self):
        return self.computer

# 指导者类
class Director:
    def __init__(self, builder):
        self.builder = builder

    def construct_computer(self):
        self.builder.build_cpu()
        self.builder.build_ram()
        self.builder.build_storage()

# 使用构建者模式创建电脑
simple_builder = SimpleComputerBuilder()
advanced_builder = AdvancedComputerBuilder()

director = Director(simple_builder)
director.construct_computer()
simple_computer = simple_builder.get_computer()
print(simple_computer)

director = Director(advanced_builder)
director.construct_computer()
advanced_computer = advanced_builder.get_computer()
print(advanced_computer)
```

## 原型模式（prototype）：通过原型的克隆创建新的实例
- 通过克隆原型来创建新的实例，从而避免了使用 new 关键字直接创建对象。
- 可以使用相同的原型，通过修改部分属性来创建新的实例
- 原型模式的核心在于提供一个接口，用于复制现有对象。通常，这个接口会包含一个 clone 方法，负责返回对象的副本。
- 用途：对于一些创建实例开销比较高的地方可以用原型模式

```python
import copy

# 定义电脑类
class Computer:
    def __init__(self, cpu, ram, storage):
        self.cpu = cpu
        self.ram = ram
        self.storage = storage

    def __str__(self):
        return f"Computer with CPU: {self.cpu}, RAM: {self.ram}GB, Storage: {self.storage}GB"

    def clone(self):
        return copy.deepcopy(self)

# 创建一个原型电脑对象
original_computer = Computer("Intel i5", 8, 256)
print("Original Computer:", original_computer)

# 通过克隆原型电脑对象创建新电脑对象
cloned_computer = original_computer.clone()
print("Cloned Computer:", cloned_computer)

# 修改克隆对象的属性
cloned_computer.cpu = "Intel i7"
cloned_computer.ram = 16

print("Modified Cloned Computer:", cloned_computer)
print("Original Computer after cloning:", original_computer)
```

## 单例模式（Borg/Singleton）：一个类只能创建同一个对象
- 一个类创建出来的对象都是同一个
- Python的模块就是单例的，只会导入一次
- 使用场景：
  - **配置管理器**：应用程序的配置通常在整个程序中是唯一的，可以使用单例模式来管理配置。
  - **日志记录器**：日志记录器通常需要在整个应用程序中共享，以确保日志记录的一致性。
  - **线程池**：线程池需要在整个应用程序中共享，以便有效地管理线程资源。


```python
class Singleton:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls._instance

    def __init__(self, value):
        # 这里可以初始化单例实例的属性
        self.value = value
# 测试单例模式
singleton1 = Singleton("First Instance")
print(singleton1.value)  # 输出: First Instance

singleton2 = Singleton("Second Instance")
print(singleton2.value)  # 输出: First Instance

# 检查两个实例是否相同
print(singleton1 is singleton2)  # 输出: True
```

## 对象池模式（Pool）：预先分配同一类型的一组实例

## 惰性计算模式（Lazing evaluation）:延迟计算

# 结构型模式
## 装饰器模式（Decorator）：无需子类化扩展对象功能

## 代理模式（Proxy）:把一个对象的操作代理到另一个对象
- 把一个对象的操作代理到另一个对象
- 通常是has-a关系
### 代理模式的作用
1. **控制访问**：可以控制对目标对象的访问。例如，只有在满足某些条件时才允许访问目标对象。
2. **延迟初始化**：可以延迟目标对象的创建，直到真正需要时才创建它，节省资源。
3. **日志记录**：可以在访问目标对象时添加日志记录功能。
4. **缓存**：可以在代理中缓存目标对象的结果，减少对目标对象的调用次数。

```python
class RealSubject:
    def request(self):
        return "RealSubject: Handling request."

class Proxy:
    def __init__(self, real_subject):
        self._real_subject = real_subject

    def request(self):
        if self.check_access():
            result = self._real_subject.request()
            self.log_access()
            return result

    def check_access(self):
        print("Proxy: Checking access prior to firing a real request.")
        return True

    def log_access(self):
        print("Proxy: Logging the time of request.")

# 测试代理模式
real_subject = RealSubject()
proxy = Proxy(real_subject)

print(proxy.request())
```
### 使用场景

- **远程代理**：用于访问远程对象，例如远程方法调用（RMI）。
- **虚拟代理**：用于按需创建资源密集型对象，例如大图像的延迟加载。
- **保护代理**：用于控制对敏感对象的访问，例如权限控制。
- **智能引用代理**：用于在访问对象时执行附加操作，例如对象的引用计数和日志记录。


## 适配器模式（Adapter）：通过一个间接层适配统一接口
- 不同对象的接口适配到同一个接口
- 相当于一个多功能充电头，可以给不同的电气充电，当了适配器
- 当我们需要给不同的对象统一接口的时候可以使用适配器模式


## 外观模式（Facade）：简化复杂对象的访问问题

## 享元模式(Flyweight):通过对象复用（池）改善资源利用，比如连接池

## Model-View-Controller（MVC）：解耦展示逻辑和业务逻辑

# 行为型模式
## 迭代器模式（Iterator）：通过统一的接口迭代对象
- Python内置对迭代器模式的支持
- 可以用for遍历各种Iterable的类型
- python里可以实现__next__,__iter__实现迭代器

```python
# 自定义栈
from collections import deque
class Stack(object):
    def __init__(self):
        self._deque = deque()
    
    def push(self, value):
        return self._deque.append(value)
    
    def pop(self):
        return self._deque.pop()
    
    def empty(self):
        return len(self._deque) == 0
    
    ## 实现迭代__iter__方法
    def __iter__(self):
        res = []
        for i in self._deque:
            res.append(i)
        for i in reversed(res)
            yield i

s = Stack()
s.push(1)
s.push(2)
for i in s:
    print(i)
# 打印2,1
```
## 观察者模式（Observer）：对象发生改变的时候，观察者执行相应的动作
- 发布订阅是一种最常见的实现方式，用于解耦逻辑
- 可以通过回调等方式实现，当事件发生时，调用相应的函数

```python
# 发布订阅模式
# 发布者
class Publisher:
    def __init__(self):
        self.observers = []  # 观察者

    def add(self, observer):
        if observer not in self.observers:
            self.observers.append(observer)
        else:
            print("Failed to add: {}".format(observer))

    def remove(self, observer):
        try:
            self.observers.remove(observer)
        except ValueError as e:
            print('Failed to remove: {}'.format(observer))

    def notify(self):  # 调用观察者的回调
        [o.notify_by(self) for o in self.observers]


class Formatter(Publisher):
    def __init__(self, name):
        super().__init__()
        self.name = name
        self._data = 0

    @property
    def data(self):
        return self._data

    @data.setter
    def data(self, new_value):
        self._data = int(new_value)
        # data在被合法赋值后执行notify
        self.notify()


# 订阅者
class BinaryFormatter:
    def notify_by(self, publisher):
        print("{}: '{}' has now bin data = {}".format(type(self).__name__, publisher.name, bin(publisher.data)))


if __name__ == "__main__":
    # 发布者
    df = Formatter('formatter')
    # 订阅者
    bf = BinaryFormatter()
    # 添加订阅者
    df.add(bf)
    # 设置的时候调用订阅者的notify_by
    df.data = 3
```

## 策略模式（Stratrgy）:针对不同规模输入使用不同的策略
- 根据不同的输入采取不同的策略
- 对外暴露统一的接口，内部采取不同的计算

```python
from abc import ABC, abstractmethod

# 策略接口（Strategy）使用抽象基类（ABC）
class PaymentStrategy(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

# 具体策略类（Concrete Strategy）
class CreditCardPayment(PaymentStrategy):
    def __init__(self, card_number):
        self.card_number = card_number

    def pay(self, amount):
        return f"Paid {amount} using Credit Card {self.card_number}."

class PayPalPayment(PaymentStrategy):
    def __init__(self, email):
        self.email = email

    def pay(self, amount):
        return f"Paid {amount} using PayPal account {self.email}."
# 上下文类（Context）
class PaymentContext:
    def __init__(self, strategy: PaymentStrategy):
        self._strategy = strategy

    def set_strategy(self, strategy: PaymentStrategy):
        self._strategy = strategy

    def execute_payment(self, amount):
        return self._strategy.pay(amount)

# 测试策略模式
credit_card_payment = CreditCardPayment("1234-5678-9876-5432")
paypal_payment = PayPalPayment("user@example.com")

context = PaymentContext(credit_card_payment)
print(context.execute_payment(100))  # 输出: Paid 100 using Credit Card 1234-5678-9876-5432.

context.set_strategy(paypal_payment)
print(context.execute_payment(200))  # 输出: Paid 200 using PayPal account user@example.com.
```
### 使用场景

- **需要动态选择算法**：在运行时需要动态选择不同的算法或行为。
- **避免条件判断**：通过策略模式，可以避免在客户端代码中使用大量的条件判断语句。
- **算法独立变化**：算法的实现细节独立于客户端代码，可以独立地进行修改和扩展。