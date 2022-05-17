# Design Patterns

## Introduction

By definition, Design Patterns are reusable solutions to commonly occuring problems(in the context of software design). Design patterns were started as best practices that were applied again and again to similar problems encountered in different contexts. They become popular after they were collected, in a formalized form, in the Gang Of Four book in 1994. Originally published with c++ and smaltalk code samples, design patterns are very popular in Java and C# can be applied in all object oriented languanges. In functional languages like Scala, certain patterns are not necesary anymore. 

## Creational Design Patterns
- [Singleton:](singleton-pattern.md) Ensures that only one instance of a class is created and provide a global access point to the object.
- [Factory (Simplified version of Factory Method):](factory-pattern.md) Creates objects without exposing the instantiation logic to the client and refers to the newly created object through a common interface. 
- [Factory Method:](factory-method-pattern.md) Defines an interface for creating objects, but let subclasses to decide which class to instantiate and refers to the newly created object through a common interface.
- [Abstract Factory:](abstract-factory-pattern.md) Offers the interface for creating a family of related objects, without explicitly specifying their classes. 
- [Builder:](builder-pattern.md) Defines an instance for creating an object but letting subclasses decide which class to instantiate and Allows a finer control over the construction process. 
- [Prototype:](prototype-pattern.md) Specifies the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.
- [Object Pool:](object-pool-pattern.md) Reuses and shares objects that are expensive to create. 

## Behavioral Design Patterns:
- [Chain of Responsibiliy:](chain-of-responsibility-pattern.md) Avoids attaching the sender of a request to its receiver, giving this way other objects the possibility of handling the request too.
<!-- 
- [责任链.md](设计模式%20-%20责任链.md)
- [命令.md](设计模式%20-%20命令.md)
- [解释器.md](设计模式%20-%20解释器.md)
- [迭代器.md](设计模式%20-%20迭代器.md)
- [中介者.md](设计模式%20-%20中介者.md)
- [备忘录.md](设计模式%20-%20备忘录.md)
- [观察者.md](设计模式%20-%20观察者.md)
- [状态.md](设计模式%20-%20状态.md)
- [策略.md](设计模式%20-%20策略.md)
- [模板方法.md](设计模式%20-%20模板方法.md)
- [访问者.md](设计模式%20-%20访问者.md)
- [空对象.md](设计模式%20-%20空对象.md)

## 四、结构型

- [适配器.md](设计模式%20-%20适配器.md)
- [桥接.md](设计模式%20-%20桥接.md)
- [组合.md](设计模式%20-%20组合.md)
- [装饰.md](设计模式%20-%20装饰.md)
- [外观.md](设计模式%20-%20外观.md)
- [享元.md](设计模式%20-%20享元.md)
- [代理.md](设计模式%20-%20代理.md) -->

## Reference
- [Design Patterns](http://www.oodesign.com/)
