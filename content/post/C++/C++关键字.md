<!--
createdAt:2023-04-21
createdBy:ycq
-->
---
title: "C++关键字"
date: 2023-04-21
tags: ["学习","C++"]
description: ""
---

### 标准C++关键字
![C++关键字](https://gitlab.com/DJVQ/image/-/raw/master/blog/%E6%A0%87%E5%87%86C++%E5%85%B3%E9%94%AE%E5%AD%97.png)
#### alignas
用来设置内存中对齐方式，最小是8字节对齐，可以是16，32，64，128等。明确规定占用字节大小后，编写代码将更具有跨平台性
##### `代码示例：`
```C++
#include <iostream>
using namespace std;
 
struct  struct_Test1
{
    char c;
    int  i;
    double d;
};
 
struct alignas(8) struct_Test2
{
    char c;
    int  i;
    double d;
};
 
struct alignas(16) struct_Test3
{
    char c;
    int  i;
    double d;
};
 
struct alignas(32) struct_Test4
{
    char c;
    int  i;
    double d;
};
 
int main()
{
    struct_Test1 test1;
    struct_Test2 test2;
    struct_Test3 test3;
    struct_Test4 test4;
 
    cout<<"char size:"<<sizeof(char)<<endl;
    cout<<"int size:"<<sizeof(int)<<endl;
    cout<<"double size:"<<sizeof(double)<<endl;
 
    cout<<"test1 size:"<<sizeof(test1)<<endl;
    cout<<"test2 size:"<<sizeof(test2)<<endl;
    cout<<"test3 size:"<<sizeof(test3)<<endl;
    cout<<"test4 size:"<<sizeof(test4)<<endl;
 
    system("pause");
 
    return 0;
}
```
##### `输出：`
```
char size:1
int size:4
double size:8
test1 size:16
test2 size:16
test3 size:16
test4 size:32
请按任意键继续. . .
```

#### asm
允许在C++程序中嵌入汇编代码

#### const
const修饰的变量必须初始化，一经赋值，不可修改
const修饰函数入参，可防止内部对其进行修改
const修饰函数返回值，可保护指针或者引用不被修改
const修饰函数体，则函数不能修改成员变量，且不能调用任何不用const修饰函数体的成员函数
const对象不能调用类中非const的成员函数

#### const_cast
形式：const_cast <_type> (expression)
_type必须是指针、引用、成员指针之一
增加/删除const或violate时，使用const_cast删除某个引用或指针的const/violate关键字时，并对其进行了修改，这是一种未定义的行为，是不安全的，反之，若能100%确定不会对其进行修改，则是安全的

#### delete
delete 和 new对应，delete[] 和 new[]对应，delete用于释放new动态申请的内存，若new分配的内存没有被delete释放掉，会发生内存泄漏；对于 new[]，若数组类型是基本数据类型或是无自定义析构函数的类类型，可以使用delete，否则必须使用 delete[]，不然会发生内存泄漏或是程序崩溃

#### dynamic_cast
形式：dynamic_cast <_type> (expression)
在运行时才处理，且运行时会进行类型检查，可保证安全性，但对编译器有要求，需要编译器开启RTTI，且执行效率相对于static_cast要差
不能用于内置的基本数据类型的强制转换
可用于对类层次见进行上下行转换，转换时基类中一定要有虚函数，否则编译不通过
当子类向父类进行转换时，一般不会出现问题；反之需要判断指针指向的对象的实际类型与转换后有的对象类型是否相同，不相同则会失败
转换成功时返回值形类的指针或引用，失败返回NULL

#### enum
```C++
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
    标识符[=整型常数]
} 枚举变量;
//example
enum color { red, green, blue } c;
c = blue;
```
#### explicit
用于修饰构造函数，禁止单参数的构造函数被用于隐式类型转换

#### export
作用与extern类似，区别是其声明的是外部模板类对象和模板函数

#### extern
声明变量或函数为外部链接，表明被声明的变量在同一文件的不同位置或者是不同文件中被定义的
C++中，extern还可以用来指定使用C语言进行链接，如：
extern "C" **
extern "C" {**}

#### inline
在函数定义时使用inline对其进行修饰，可将其数据放在栈中，对于一些简单的、开销小的、但需要频繁调用的函数，使用inline可提高程序执行效率，但相对的内存开销变大，需要慎用
inline函数不能包含switch、while等复杂的控制语句，并且其本身不能是递归的
inline函数的定义被建议放在头文件中
类中的成员函数定义时默认时内联的，但若定义在外部则默认不是内联的，需要加上inline修饰才能变为内联

#### mutable
只能作用于类的非静态和非常量数据成员，表示无论如何其是可变的。被const修饰函数体的函数可以修改被mutable修饰的数据变量

#### register
对于被register修饰的变量，编译器会尽量把它存储在CPU内部的寄存器中，而不是内存，提高访问效率。因此，被register修饰的变量必须是寄存器可接受的类型，能否使用有时候取决于机器

#### reinpreter_cast
形式：reinpreter_cast <_type> (expression)
并不会修改运算对象本身，而是对该对象从位模式上进行重新解释
作用是更改对指针指向的内容的解释，很容易带来风险
reinterpret_cast的一个实际用途是在哈希函数中，即通过让两个不同的值几乎不以相同的索引结尾的方式将值映射到索引

#### static_cast
在编译时进行，不能更改const、violate、__unaligned
与dynamic_cast类似，可用于父类与子类之间指针或引用的上下行转换，其中上行转换是安全的，下行转换由于不进行动态类型检查，是不安全的
可用于基本数据类型之间的转换
可转换空指针的类型

#### template
模板，主要用于实现泛型机制

#### typeof
定义一种数据类型的新名字，也就是别名

#### typeid
获取表达式或数据类型的类型信息，与dynamic_cast一样需要编译器开启RTTI
返回类型为type_info，对于type_info，若表达式的类型是类且包含虚函数，那么typeid返回表达式的动态类型
iso C++标准规定typeid必须实现四种操作：==、!=、name()、before()

#### typename
在模板中，用于表示一个参数类型，用于实现泛型机制，与class作用一样，但是使用class有时会造成混淆，所以使用typename专门表示修饰的参数为类型名称而不是别的成员函数或成员变量

#### union
```C++
union tag {member-list};
```
`tag`为`union`提供的类型名称
`member-list`为`union`可以包含的成员
`union`是一个用户定义类型，其中所有成员都共享同一个内存位置
如果是多个变量的话，`union`的大小就为最大的那一个变量
`union`无法自动释放```new```出来的内存，比如重复对`char*`类型赋值，被覆盖的`char*`的内存没有被释放
```C++
struct var{
    union {
        int iv;
        double dv;
        char* sv;
    };
    var(const int& v) :iv{ v } {};
    var(const double& v) :dv{ v } {};
    var(const char* s) {
        int len = strlen(s);
        sv = new char[len + 1];
        memcpy(sv, s, len + 1);
    }
};
int main()
{
    var x = 1415;
    cout << x.iv << endl;
    x = 3.14;
    cout << x.dv << endl;
    x = "hello world";
    cout << x.sv << endl;
}
```
```
1415
3.14
hello world
```

#### violate
可用于声明可在程序中由硬件修改的对象的类型限定符
```C++
volatile declarator ;
```
* __易变性__：在汇编层⾯来看，对于两条连续的语句，下⼀条语句不会直接使⽤上⼀条语句对应的
`volatile`变量的寄存器内容，⽽是重新从内存中读取
* __不可优化性__：`volatile`告诉编译器，不要对变变量进⾏各种激进的优化，甚⾄将变量直接
消除，保证写在代码中的指令，⼀定会被执⾏
* __顺序性__：保证`volatile`变量之间的顺序，编译器不会进⾏乱序优化(禁止指令重排序)

#### virtual
用于保证C++的多态性，父类中被virtual修饰的函数可被子类覆盖
父类中被virtual修饰的函数，且不对其进行定义，直接令其=0，这种函数称为纯虚函数，这样的父类为抽象类，抽象类不能被实例化，继承抽象类的子类必须重新重写抽象类的纯虚函数，否则可将继承到的纯虚函数也定义为纯虚函数，这样子类也变成了抽象类
子类继承抽象父类时使用virtual修饰对应的父类时，可解决多继承是的命名冲突和冗余数据的问题

#### wchar_t
表示宽字符类型，每个wchar_t类型占2个char，可用于表示汉字
