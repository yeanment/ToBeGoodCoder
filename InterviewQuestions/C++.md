# 基本语言特性

## static关键字的作用
同时编译多个文件时，所有未加static前缀的全局变量和函数都具有全局可见性，所以加了static关键字的变量和函数可对其它源文件隐藏，还可以保持变量内容的持久性。
用static前缀作为关键字的变量默认的初始值为0。

## 虚函数和纯虚函数的区别
含有纯虚函数的类称为抽象类，只含有虚函数的类不能称为抽象类。虚函数可以直接被使用，也可以被子类重载以后以多态形式调用，而纯虚函数必须在子类中实现该函数才可使用，因为纯虚函数在基类中只有声明而没有定义。虚函数必须实现,对虚函数来说父类和子类都有各自的版本。

纯虚函数定义

```C++
virtual TypeName FunctionName(args) = 0;
```

## 在程序里面智能指针的名字
一个是shared_ptr允许多个指针指向同一个对象；一个是unique_ptr独占所指向的对象；还有一种伴随类weak_ptr，他是一种弱引用，指向shared_ptr所指向的对象。

## new和malloc区别
+ malloc与free是C++/C语言的标准库函数，new/delete是C++的运算符。它们都可用于申请动态内存和释放内存。

+ 对于非内部数据类型的对象而言，光用maloc/free无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加于malloc/free。

+ 因此C++语言需要一个能完成动态内存分配和初始化工作的运算符new，以一个能完成清理与释放内存工作的运算符delete。注意new/delete不是库函数。

+ C++程序经常要调用C函数，而C程序只能用malloc/free管理动态内存

## 函数后面接const
这是把整个函数修饰为const，意思是“函数体内不能对成员数据做任何改动”。如果你声明这个类的一个const实例，那么它就只能调用有const修饰的函数

## 函数指针
```C++
int(*func)(int, int)
```

# C++ Standard Library

## To be read
+ C++标准库，包括了STL容器，算法和函数等。
+ C++ Standard Library：是一系列类和函数的集合，使用核心语+ 言编写，也是C++ISO自身标准的一部分。
+ Standard Template Library：标准模板库
+ C POSIX library：POSIX系统的C标准库规范
+ ISO C++ Standards Committee：C++标准委员会
