---
title: "C与C++"
date: 2023-04-28
tags: ["学习","C++"]
description: ""
---

#### 语法和关键字
* `C`中头文件与`C++`不同，如`math.h`和`cmath`
* `C`中命名空间与`C++`不同，如`std`
* `C`中的关键字如`malloc`，`free`在`C++`中进一步封装成`new`、`delete`；`C++`中的`auto`、`explicit`、`dynamic_cast`等关键字在`C`中不存在

#### 函数
* `C++`中的重载和虚函数的概念在`C`中是不存在的，`C++`函数名字的修饰会将参数加在后⾯，`int func(int,double)`经过名字修饰之后会变成`_func_int_double`，⽽`C`中则会变成`_func`

#### 函数指针与function
* 函数指针时`C`和`C++`中都有的，而`function`仅存在于`C++`
* 函数指针是一种指向函数的指针，与指针函数是两个概念
* 函数指针的声明写法是```[rerurn_type] (*func_name)(arg_type...)```，其中```[rerurn_type] (* )(arg_type...)```可看函数指针类型，```func_name```可看作函数指针名
* ```function```是一种模板类，其的声明方法是```function<_Res(_ArgTypes...)> func```，其中```_Res```为函数返回类型，```_ArgTypes```为入参类型，```function```可实现函数指针的功能
* 由于```function```是一个类，所以其可以重载运算符，如`()`、`=`等，相较于函数指针功能更为强大

#### 动态内存
![image](https://gitlab.com/DJVQ/image/-/raw/master/blog/C++%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84.png)
其中，当函数被调用时，将对应的连续内存(堆栈帧)入栈，当函数返回时，将堆栈帧弹出
堆栈帧的结构：
![image](https://gitlab.com/DJVQ/image/-/raw/master/blog/C++%E6%A0%88%E5%B8%A7%E7%AE%80%E7%95%A5.png)
* `C++`中使用`new`来动态申请内存，一个对象若直接创建，在栈中为其分配内存空间，否则在堆中分配内存空间，且再分配的空间上调用其构造函数，这也是`new`与`C`中的`malloc`的主要区别；每次使用`new`创建新的对象时，最后需要调用`delete`来清除，`delete`操作首先调用对象的析构函数，在释放其对应的空间
实际上对于一些简单的、微小的、运行时间短`C++`程序，不使用`delete`释放`new`分配的空间，程序运行结束时，操作系统也会进行清理
* `new`操作实际上使用了`operator new`和`placement new`两个方法，`operator new`方法实际上是使用`malloc`来分配空间，`placement new`则是调用类的构造函数；对应的也有`operator delete`，其使用`free`来释放空间，`placement delete`则调用类的析构函数。此外，每个类可以重载`operator new/delete`，但需要慎用

#### class与struct

`C`的`struct`:
```C
struct [struct_name]
{
   member-list
} [struct_variable];
[struct] tag declarators;
```
* `struct`是一种数据类型，不能在其内部定义函数
* `struct`中对内部成员的访问权限有且只有`public`
* `struct`不可继承

`C++`的`struct`：
```C++
[template-spec] struct [ms-decl-spec] [tag [: base-list ]]
{
   member-list
} [declarators];
[struct] tag declarators;
```
* `struct`与`class`大致一样；
* `class`中的成员默认属性时`private`，而`struct`中为`public`；
* `class`中的继承默认是`private`继承，而`struct`中为`public`继承；
* `struct`可继承`class`


`C++`的`class`：
```C++
[template-spec]
class [ms-decl-spec] [tag [: base-list ]]
{
   member-list
} [declarators];
[ class ] tag declarators;
```
* `class`可定义成员变量以及成员方法，并且用`public`、`private`、`protected`修饰；
* `class`支持多继承

#### 模板
* `C++`中增加了模板，用以重⽤代码，提供了更加强⼤的`STL`标准库

#### 总结
* `C`是⼀种结构化的语⾔，重点在于算法和数据结构。`C`程序的设计⾸先考虑的是如何通过⼀个代码，⼀个过程对输⼊进⾏运算处理输出。⽽`C++`⾸先考虑的是如何构造⼀个对象模型，让这个模型能够契合与之对应的问题领域，这样就能通过获取对象的状态信息得到输出