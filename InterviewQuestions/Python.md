# 基本语言特性
## 生成器和迭代器
python生成器是一个返回可以迭代对象的函数,可以被用作控制循环的迭代行为。生成器类似于返回值为数组的一个函数,这个函数可以接受参数,可以被调用,一般的函数会返回包括所有数值的数组,生成器一次只能返回一个值,这样消耗的内存将会大大减小。

## is和==的区别
+ is用来判断两个变量引用的对象是否为同一个
+ ==用于判断引用对象的值是否相等
+ id()函数查看引用对象的地址。

## 方法解析顺序
Python的方法解析顺序优先级从高到低为:实例本身类继承类(继承关系越近,越先定义,优先级越高) 

**Method resolution order**: In Python, every class whether built-in or user-defined is derived from the object class and all the objects are instances of the class object. Hence, object class is the base class for all the other classes. In the case of multiple inheritance **a given attribute is first searched in the current class** if it’s not found then it’s searched in the parent classes. **The parent classes are searched in a depth-first, left-right fashion and each class is searched once.**

If we see the above below then the order of search for the attributes will be Derived, Base1, Base2, object. The order that is followed is known as a linearization of the class Derived and this order is found out using a set of rules called Method Resolution Order (MRO)


**super() function with multilevel inheritance**: The Python super() function allows us to refer the superclass implicitly, which has a property that it always refers **the immediate superclass**. Also, super() function is not only referring the \_\_init\_\_() but it can also call the other functions of the superclass when it needs.


## Instance Method vs ClassMethod vs StaticMethod
### Instance Method
Instance methods can freely access attributes and other methods on the same object. This gives them a lot of power when it comes to modifying an object’s state. Not only can they modify object state, instance methods can also access the class itself through the ```self.__class__```attribute. This means instance methods can also modify class state.

We were able to call *ClassMethod* or *StaticMethod* on  the class itself just fine, but attempting to call the *InstanceMethod* failed with a ```TypeError```.
### Class Method
Instead of accepting a ```self``` parameter, class methods take a cls parameter that points to the class—and not the object instance—when the method is called.Because the class method only has access to this ```cls``` argument, it can’t modify object instance state. That would require access to ```self```. However, class methods can still modify class state that applies across all instances of the class.

Python only allows one ```__init__``` method per class. Using class methods it’s possible to add as many alternative constructors as necessary. This can make the interface for your classes self-documenting (to a certain degree) and simplify their usage.
### Static Method
This type of method takes neither a ```self``` nor a ```cls``` parameter (but of course it’s free to accept an arbitrary number of other parameters).Therefore a static method can neither modify object state nor class state. Static methods are restricted in what data they can access - and they’re primarily a way to namespace your methods.
```Python
from datetime import date 
class Person: 
    class_var = 12
    def __init__(self, name, age): 
        self.name = name 
        self.age = age 
       
    # a class method to create a Person object by birth year. 
    @classmethod
    def fromBirthYear(cls, name, year): 
        return cls(name, date.today().year - year) 
        
    @classmethod
    def change_class_var(cls, val):
        cls.class_var = val
       
    # a static method to check if a Person is adult or not. 
    @staticmethod
    def isAdult(age): 
        return age > 18
   
person1 = Person('mayank', 21) 
person2 = Person.fromBirthYear('mayank', 1996) 
   
print (person1.age) 
print (person2.age) 
   
# print the result 
print (Person.isAdult(22)) 

a = Person("a", 1)
b = Person("b", 2)
print(a.class_var)
print(b.class_var)
Person.change_class_var(2)
print(a.class_var)
print(b.class_var)
```

## Dunder Methods / Magic Methods
Dunder or magic methods in Python are the methods having two prefix and suffix underscores in the method name. Dunder here means "Double Under (Underscores)". These are commonly used for operator overloading. Few examples for magic methods are: ```__init__```, ```__add__```, ```__len__```, ```__repr__``` etc.

Please refer to the [blog](https://levelup.gitconnected.com/python-dunder-methods-ea98ceabad15
) for more information.

## dict和list的区别
+ dict查找速度快，占用的内存较大，dict不能用来存储有序集合，用{}表示。dict是通过hash表实现的,dict为一个数组,数组的索引键是通过hash函数处理后得到的,hash函数的目的是使键值均匀的分布在数组中。
+ list查找速度慢，占用内存较小，用[]表示。

## 深拷贝，浅拷贝
深复制和浅复制最根本的区别在于是否是真正获取了一个对象的复制实体，而不是引用。
+ 浅复制：只是拷贝了基本类型的数据，而引用类型数据，复制后也是会发生引用，浅复制仅仅是指向被复制的内存地址，如果原地址中对象被改变了，那么浅复制出来的对象也会相应改变。
+ 深复制：在计算机中开辟了一块新的内存地址用于存放复制的对象。

## 多进程
+ os.fork()
+ 使用multiprocessing模块：创建Process的实例，传入任务执行函数作为参数；派生Process的子类，重写run方法
+ 使用进程池Pool

## 各种锁

### 全局解释器锁（GIL）
> 每个CPU在同一时间只能执行一个线程，那么其他的线程就必须等待该线程的全局解释器，使用权消失后才能使用全局解释器，即使多个线程直接不会相互影响在同一个进程下也只有一个线程使用cpu，这样的机制称为全局解释器锁（GIL）。GIL的设计简化了CPython的实现，使的对象模型包括关键的内建类型，如：字典等，都是隐含的，可以并发访问的，锁住全局解释器使得比较容易的实现对多线程的支持，但也损失了多处理器主机的并行计算能力。
>
> 全局解释器锁避免了大量的加锁解锁的好处，使数据更加安全，解决多线程间的数据完整性和状态同步。但同时，它使多核处理器退化成单核处理器，只能并发不能并行。
>
> GIL一般适用于多线程情况下必须存在资源的竞争，保证在解释器级别的线程唯一使用共享资源（cpu）。

### 同步锁
> 同一时刻的一个进程下的一个线程只能使用一个cpu，要确保这个线程下的程序在一段时间内被cpu执行，那么就要用到同步锁。因为有可能当一个线程在使用cpu时，该线程下的程序可能会遇到io操作，那么cpu就会切到别的线程上去，这样就有可能会影响到该程序结果的完整性。只需要在对公共数据的操作前后加上上锁和释放锁的操作即可使用同步锁。同步锁保证解释器级别下的自己编写的程序唯一使用共享资源产生了同步锁。

### 死锁
> 死锁指两个或两个以上的线程或进程在执行程序的过程中，因争夺资源或者程序推进顺序不当而相互等待的一个现象。死锁产生的必要条件：互斥条件、请求和保持条件、不剥夺条件、环路等待条件。处理死锁的基本方法：预防死锁、避免死锁（银行家算法）、检测死锁（资源分配）、解除死锁（剥夺资源、撤销进程）。

### 递归锁
> 在Python中为了支持同一个线程中多次请求同一资源，Python提供了可重入锁。这个RLock内部维护着一个Lock和一个counter变量，counter记录了acquire的次数，从而使得资源可以被多次require。直到一个线程所有的acquire都被release，其他的线程才能获得资源。递归锁分为可递归锁与非递归锁。

### 乐观锁
> 假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性。

### 悲观锁
> 假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作。

### 常用的加锁方式
> 互斥锁、可重入锁、迭代死锁、互相调用死锁、自旋锁。


# PyTorch 相关
## pytorch中cuda()作用
+ cuda()将操作对象放在GPU内存中，加了cuda()的Tensor放在GPU内存中
+ 没加cuda()的Tensor放在CPU内存中

