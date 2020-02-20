# Python高级 - 类

## 1. `super()`

super(cls, instance)返回的对象支持在基类上执行属性查找。如果使用该函数，Python将使用基类上的搜索规则类搜索属性。Python3中使用super()



## 2. 多重继承搜索的顺序

* 基类顺序由C3线性优化算法决定，算法论文：《A Monotonic Superclass Linearization for Dylan》（K. Barrent等，OOPSLA'96）
* 某些类层次结构将被Python拒绝并抛出TypeError。示例代码如下：

```python
class A: pass
class B(A): pass
class C(A, B): pass
```

* 通过`ClassName.__mro__`查看基类顺序



## 3. 私有类型实现

类中所有以双下划线开头的名称（如`__foo`）会自动变形为`_ClassName__foo`形式的名称，以防止继承后属性或方法产生冲突



## 4. `__new__()`

* 原型：`__new__(cls, *args, **kwargs)`, 返回一个实例

* 创建类的实例时，会先调用`__new__()`创建新实例，然后使用`__init__()`初始化实例。操作`f = Foo(2)`时会执行：

```python
f = Foo.__new__(Foo, 2)
if isinstance(f, Foo):
    Foo.__init__(f, 2)
```

* 用途：
  * 继承的基类是不可变的如str
  * 定义元类

```python
class UpperStr(str):
    def __new__(cls, value=""):
        return str.__new__(cls, value.upper())
```



## 5. `__del__()`

当程序的引用计数变为0时会调用`__del__()`， 但是del语句不会直接调用`__del__()`方法。

**风险**：<mark>**定义了`__del__()`的实例无法被Python的循环垃圾回收器回收**</mark>



## 6. 抽象基类

```python
from abc import ABCMeta, abstractclassmethod, abstractproperty

class A(metaclass=ABCMeta):

    @abstractclassmethod
    def haha(self):
        pass

    @abstractproperty
    def name(self):
        pass

```

`class A(metaclass=ABCMeta):`定义A是一个抽象基类，继承自抽象基类的类必须实现`@abstractclassmethod`和`@abstractproperty`所装饰的内容。

* `@abstractclassmethod` 定义了子类必须实现的方法
* `@abstractproperty`定义了子类必须实现的`property`属性



## 7. 元类

元类用于显著的更改用户定义类的行为和属性。python3中默认的元类是`type()`，python2中默认的元类是`types.ClassType`即旧式类。

