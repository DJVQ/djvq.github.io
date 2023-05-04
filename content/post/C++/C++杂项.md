<!--
createdAt:2023-04-20
createdBy:ycq
-->
---
title: "C++杂项"
date: 2023-04-20
tags: ["学习","C++"]
description: ""
---

### 指针和引用
###### 示例程序
```C++
#include <iostream>
using namespace std;

void change1(int n) {
    cout << "值传递--函数操作地址" << &n << endl;
    n++;
}
void change2(int& n) {
    cout << "引用传递--函数操作地址" << &n << endl;
    n++;
}
void change3(int* n) {
    cout << "指针传递--函数操作地址 " << n << endl;
    *n = *n + 1;
}
void swap1(int* x, int* y) {
    cout << "交换前, x的值=" << x <<", y的值=" << y << endl;
    int* temp;
    temp = x;
    x = y;
    y = temp;
    cout << "交换后, x的值=" << x << ", y的值=" << y << endl;
}
void swap2(int* x, int* y) {
    cout << "交换前, x的值=" << x << ", y的值=" << y << endl;
    int temp = *x;
    *x = *y;
    *y = temp;
    cout << "交换后, x的值=" << x << ", y的值=" << y << endl;
}
void swag3(int& x, int& y) {
    cout << "交换前, x的值=" << x << ", y的值=" << y << endl;
    int temp = x;
    x = y;
    y = temp;
    cout << "交换后, x的值=" << x << ", y的值=" << y << endl;
}
int main(){
    int n = 10;
    int m = 11;
    cout << "实参的地址" << &n << endl;
    change1(n);
    cout << "after change1() n=" << n << endl;
    change2(n);
    cout << "after change2() n=" << n << endl;
    change3(&n);
    cout << "after change3() n=" << n << endl;
    n = 10;
    cout << "swag1前, n的值=" << n << ", m的值=" << m << endl;
    swap1(&n, &m);
    cout << "swag1后, n的值=" << n << ", m的值=" << m << endl;
    n = 10;
    m = 11;
    cout << "swag2前, n的值=" << n << ", m的值=" << m << endl;
    swap2(&n, &m);
    cout << "swag2后, n的值=" << n << ", m的值=" << m << endl;
    n = 10;
    m = 11;
    cout << "swag3前, n的值=" << n << ", m的值=" << m << endl;
    swag3(n, m);
    cout << "swag3后, n的值=" << n << ", m的值=" << m << endl;
    return 0;
}
```
###### 输出
```
实参的地址0000003C7B39FAB4
值传递--函数操作地址0000003C7B39FA90
after change1() n=10
引用传递--函数操作地址0000003C7B39FAB4
after change2() n=11
指针传递--函数操作地址 0000003C7B39FAB4
after change3() n=12
swag1前, n的值=10, m的值=11
交换前, x的值=0000003C7B39FAB4, y的值=0000003C7B39FAD4
交换后, x的值=0000003C7B39FAD4, y的值=0000003C7B39FAB4
swag1后, n的值=10, m的值=11
swag2前, n的值=10, m的值=11
交换前, x的值=0000003C7B39FAB4, y的值=0000003C7B39FAD4
交换后, x的值=0000003C7B39FAB4, y的值=0000003C7B39FAD4
swag2后, n的值=11, m的值=10
swag3前, n的值=10, m的值=11
交换前, x的值=10, y的值=11
交换后, x的值=11, y的值=10
swag3后, n的值=11, m的值=10
```
***
### 智能指针
说明：
智能指针是在堆栈上声明的类模板，并可通过使用指向某个堆分配的对象的原始指针进行初始化。 在初始化智能指针后，它将拥有原始的指针。 这意味着智能指针负责删除原始指针指定的内存。 智能指针析构函数包括要删除的调用，并且由于在堆栈上声明了智能指针，当智能指针超出范围时将调用其析构函数，尽管堆栈上的某处将进一步引发异常
通过使用熟悉的指针运算符（`->` 和 `*`）访问封装指针，智能指针类将重载这些运算符以返回封装的原始指针
C++ 智能指针思路类似于在语言（如 C#）中创建对象的过程：创建对象后让系统负责在正确的时间将其删除。 不同之处在于，单独的垃圾回收器不在后台运行；按照标准 C++ 范围规则对内存进行管理，以使运行时环境更快速更有效
#### unique_ptr
只允许基础指针的一个所有者。 除非需要`shared_ptr`，否则请将该指针用作纯旧 C++ 对象 (POCO) 的默认选项。 可以移到新所有者，但不会复制或共享。 替换已弃用的`auto_ptr`。 与`boost::scoped_ptr`比较。 `unique_ptr`小巧高效；大小等同于一个指针且支持`rvalue`引用，从而可实现快速插入和对 C++ 标准库集合的检索。 
`auto_ptr`功能与`unique_ptr`类似，但是其对于以下情况时合法的：
```C++
auto_ptr<std::string> p1 (new string ("hello"));
auto_ptr<std::string> p2;
p2 = p1; //auto_ptr 不会报错.
```
p2 剥夺了 p1 的所有权，但是当程序运行时访问 p1 将会报错。所以`auto_ptr`
的缺点是：存在潜在的内存崩溃问题，将其替换为`unique_ptr`，上面的代码无法通过编译
![unique_ptr](https://gitlab.com/DJVQ/image/-/raw/master/blog/unique_ptr.png)
#### shared_ptr
采用引用计数的智能指针。 如果想要将一个原始指针分配给多个所有者（例如，从容器返回了指针副本又想保留原始指针时），使用该指针。 直至所有`shared_ptr`所有者超出了范围或放弃所有权，才会删除原始指针。 大小为两个指针；一个用于对象，另一个用于包含引用计数的共享控制块
![shared_ptr](https://gitlab.com/DJVQ/image/-/raw/master/blog/shared_ptr.png)
#### weak_ptr
结合`shared_ptr`。`weak_ptr`提供对一个或多个`shared_ptr`实例拥有的对象的访问，但不参与引用计数。如果仅需要观察某个对象但不需要其保持活动状态，可使用该实例
当最后一个管理资源的`shared_ptr`对象被销毁时，即使存在指向资源的`weak_ptr`对象，资源也将被释放
在某些情况下，需要断开`shared_ptr`实例间的循环引用，比如对于一个循环链表，三个元素互相连接形成闭环，每个元素均包含下一个元素的对象，这种情况下无法释放节点，当将其中一个节点替换为`weak_ptr`时，`weak_ptr`实力能够成功被释放，它没有对内存的控制权，其生命周期结束时也就不会影响其指向的内存，因此不会与`shared_ptr`互相引用形成闭环
`weak_ptr`对象不提供直接访问其所指向的资源的权限。需要使用资源的代码可通过一个`shared_ptr`对象来执行该操作，该对象通过调用成员函数`lock`创建并拥有资源。 `weak_ptr`对象在其所指向的资源被释放时已过期，因为所有拥有该资源的`shared_ptr`对象已被销毁。调用已过期的`weak_ptr`对象上的`lock`将创建一个空`shared_ptr`对象。
![weak_ptr](https://gitlab.com/DJVQ/image/-/raw/master/blog/weak_ptr.png)
***
### 内存分配
![内存结构](https://gitlab.com/DJVQ/image/-/raw/master/blog/C%2B%2B%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84.png)
`Stack`：由编译器管理分配和回收，存放局部变量和函数参数
`Heap`：由开发者管理，需要使用 `new` `malloc` `delete` `free` 进⾏分配和回收，空间较⼤，但可能会出现内存泄漏和空闲碎⽚的情况
`BSS`：全局/静态存储区，分为初始化和未初始化两个相邻区域，存储初始化和未初始化的全局变量和静态变量
`Data Segment`：存储常量，⼀般不允许修改
`Code Segment`：存放程序的二进制代码
![堆栈帧](https://gitlab.com/DJVQ/image/-/raw/master/blog/C++%E6%A0%88%E5%B8%A7%E7%AE%80%E7%95%A5.png)
`Stack Frame`：栈帧，是一个堆栈的抽象，是编译器用来实现过程/函数调用的一种数据结构。栈帧利用`EBP`寄存器(堆栈指针)访问局部变量、参数、函数返回地址等，提供线程执行的上下文信息。线程始终在函数中执行，而堆栈帧保存的是函数的局部变量和参数信息，每一次函数调用，会调用栈上的一个栈帧。一个独立的栈帧一般包括：

- 函数的返回地址和参数
- 临时变量：包括函数的非静态局部变量以及编译器自动生成的其他临时变量
- 函数调用的上下文

栈是从高地址向低地址延伸，一个函数的栈帧用`EBP`和`ESP`这两个寄存器来划定范围。`EBP`指向当前栈帧的底部，`ESP`始终指向栈帧的顶部
`EBP`寄存器又被称为帧指针（Frame Pointer）
`ESP`寄存器又被称为栈指针（Stack Pointer）
![栈帧](https://gitlab.com/DJVQ/image/-/raw/master/blog/C++%E6%A0%88%E5%B8%A7.png)
