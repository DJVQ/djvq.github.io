<!--
createdAt:2023-04-13
createdBy:ycq
-->
---
title: "C++基础"
date: 2023-04-13
tags: ["学习","C++"]
description: ""
---

### 编译器
##### 功能
`RTTI`（`Run-Time Type Identification`)-运行时类型识别

### C++基本数据类型
`micarosoft C++`:
![image](https://learn.microsoft.com/zh-cn/cpp/cpp/media/built-intypesizes.png?view=msvc-170)
不同平台比较：
![image](https://www.runoob.com/wp-content/uploads/2014/09/32-64.jpg)
#### float
![image](https://www.runoob.com/wp-content/uploads/2014/09/v2-749cc641eb4d5dafd085e8c23f8826aa_hd.png)
#### double
![imag](https://www.runoob.com/wp-content/uploads/2014/09/v2-48240f0e1e0dd33ec89100cbe2d30707_hd.png)

### C++常量
#### 整数常量
前缀：`0x`、`0X`表示十六进制；`0`表示八进制；无前缀表示十进制
后缀：`u`、`U`表示无符号整数；`l`、`L`表示长整数

#### 浮点常量
使不使用小数点表示时，可使用[int][e|E][int]表示

#### 字符常量
括在单引号中，如果以L开头，则表示一个宽字符常量，例如：`L'x'`，此时必须存储在`wchar_t`类型的变量中。否则，是一个窄字符常量，例如：`'x'`，此时存储在`char`类型的简单变量中
字符常量可以是一个普通的字符（例如 `'x'`）、一个转义序列（例如 `'\t'`），或一个通用的字符（例如 `'\u02C0'`）

#### 字符串常量
`string`的常量为"`[string]`"表示，可使用\来换行
```C++
string hw = "hello, \
                   word";
```
#### 定义常量
`#define`和`const`
`#define`不能指定类型，`const`可以

### C++面向对象
#### C++类
##### 访问修饰符和继承
`public`：类的外部可访问
`private`：类的外部不可见和不可访问，类的友元函数可访问
`protected`：类的子类可访问
`public`继承：父类`public`，`protected`，`private`成员的访问属性在子类中为：`public`, `protected`, `private`
`protected`继承：父类 `public`，`protected`，`privat`e成员的访问属性在子类中为：`protected`, `protected`, `private`
`private`继承：父类`public`，`protected`，`private`成员的访问属性子类中为：`private`, `private`, `private`
继承形式：`class [derived_class_name]: [access_specifier] [base_class_name]`
`derived_class_name`为子类名，`access_specifier`即访问修饰符，有`public`、`protected`、`private`三种，`base_class_name`为父类名

##### 重载
函数：指参数的个数、类型或者顺序至少有有一个不同，不能仅通过返回类型的不同来重载函数
运算符：形式为`[return] operator[symbol] (parameter0,...,parameterN)`，其中`symbol`不可以是`.、.*、->*、::、sizeof、?:、#`

##### 多态
```C++
Father a ;          // 父类对象  
Son b ;             // 子类对象  

a = b ;             // 可以  
b = a ;             // 不可以  

Father *pa = &b ;   // 可以  
Son *pb = &a ;      // 不可以  

Father &f = b ;     // 可以  
Son &s = a ;        // 不可以   
```
若父类与子类有相同的方法，那么使用子类赋值的父类调用对应的方法还是其本身的方法，除非父类对应方法使用`virtual`修饰

### C++标准库
#### 文件和流
读取：`ifstream`
写入：`oftream`
读/写：`fstream`

#### 异常处理
![image](https://www.runoob.com/wp-content/uploads/2015/05/exceptions_in_cpp.png)
```C++
#include <iostream>
#include <exception>
using namespace std;
 
struct MyException : public exception
{
  const char * what () const throw ()
  {
    return "C++ Exception";
  }
};
 
int main()
{
  try
  {
    throw MyException();
  }
  catch(MyException& e)
  {
    std::cout << "MyException caught" << std::endl;
    std::cout << e.what() << std::endl;
  }
  catch(std::exception& e)
  {
    //其他的错误
  }
}
```

#### 命名空间
当中大型程序有多个人开发时，可能会出现变量名重复的情况，可使用命名空间加以区分
```C++
namespace Li{  //小李的变量定义
    FILE* fp = NULL;
}
namespace Han{  //小韩的变量定义
    FILE* fp = NULL;
}

using Li::fp;
fp = fopen("one.txt", "r");  //使用小李定义的变量 fp
Han :: fp = fopen("two.txt", "rb+");  //使用小韩定义的变量 fp
```

#### 模板
类似于`Java`中的泛型
##### 函数模板
格式：
```C++
template<parameter-list> 
//function-declaration
```
举例：
```C++
//template1.cpp #include <iostream>
template<typename T> 
void swap(T &a, T &b) {
    T tmp{a}; a = b;
    b = tmp;
}
int main(int argc, char* argv[]){
    int a = 2; int b = 3;
    swap(a, b); // 使用函数模板
    std::cout << "a=" << a << ", b=" << b << std::endl;
    double c = 1.1; 
    double d = 2.2; 
    swap(c, d);
    std::cout<<"c="<<c<<", d="<<d<<std::endl;
    return 0;
}
```
函数模板不是函数，只有当使用具体的类型替换模板中的类型的时候，才会生成具体函数，也就是函数模板的实例化。
##### 类模板
格式：
```C++
template <class type>
class class_name{
    ...
}
```
举例：
```C++
#include <iostream>
#include <vector>
#include <cstdlib>
#include <string>
#include <stdexcept>
 
using namespace std;
 
template <class T>
class Stack { 
  private: 
    vector<T> elems;     // 元素 
 
  public: 
    void push(T const&);  // 入栈
    void pop();               // 出栈
    T top() const;            // 返回栈顶元素
    bool empty() const{       // 如果为空则返回真。
        return elems.empty(); 
    } 
}; 
 
template <class T>
void Stack<T>::push (T const& elem) 
{ 
    // 追加传入元素的副本
    elems.push_back(elem);    
} 
 
template <class T>
void Stack<T>::pop () 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::pop(): empty stack"); 
    }
    // 删除最后一个元素
    elems.pop_back();         
} 
 
template <class T>
T Stack<T>::top () const 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::top(): empty stack"); 
    }
    // 返回最后一个元素的副本 
    return elems.back();      
} 
 
int main() 
{ 
    try { 
        Stack<int>         intStack;  // int 类型的栈 
        Stack<string> stringStack;    // string 类型的栈 
 
        // 操作 int 类型的栈 
        intStack.push(7); 
        cout << intStack.top() <<endl; 
 
        // 操作 string 类型的栈 
        stringStack.push("hello"); 
        cout << stringStack.top() << std::endl; 
        stringStack.pop(); 
        stringStack.pop(); 
    } 
    catch (exception const& ex) { 
        cerr << "Exception: " << ex.what() <<endl; 
        return -1;
    } 
}
```

#### 预处理
常用的预处理命令：
```C++
#define            //定义一个预处理宏
#undef             //取消宏的定义

#if                //编译预处理中的条件命令, 相当于C语法中的if语句
#ifdef             //判断某个宏是否被定义, 若已定义, 执行随后的语句
#ifndef            //与#ifdef相反, 判断某个宏是否未被定义
#elif              //若#if, #ifdef, #ifndef或前面的#elif条件不满足, 则执行#elif之后的语句, 相当于C语法中的else-if
#else              //与#if, #ifdef, #ifndef对应, 若这些条件不满足, 则执行#else之后的语句, 相当于C语法中的else
#endif             //#if, #ifdef, #ifndef这些条件命令的结束标志.
defined            //与#if, #elif配合使用, 判断某个宏是否被定义

#include           //包含文件命令
#include_next      //与#include相似, 但它有着特殊的用途

#line              //标志该语句所在的行号
#                  //将宏参数替代为以参数值为内容的字符窜常量
##                 //将两个相邻的标记(token)连接为一个单独的标记
#pragma            //说明编译器信息

#warning           //显示编译警告信息
#error             //显示编译错误信息
```
`C++ `中预定义的宏：
```C++
__LINE__	//在程序编译时包含当前行号
__FILE__	//在程序编译时包含当前文件名
__DATE__	//包含一个形式为 month/day/year 的字符串，它表示把源文件转换为目标代码的日期。
__TIME__	//包含一个形式为 hour:minute:second 的字符串，它表示程序被编译的时间。
```

##### define与const的区别
`define`：在预编译阶段进⾏处理，没有类型和类型检查，程序运行时不会为宏定义分配内存。从汇编的⻆度来讲，`define`以⽴即数的⽅式保留了多份数据的拷⻉
`const`：在编译期间进⾏处理，有类型和类型检查，程序运行时系统会为`const`常量分配内存。从汇编的角度来讲，`const`常量出现的地方保留的是数据的内存地址，只保留了一份数据的拷贝，省去了不必要的内存空间。此外，有时编译器不会为普通的`const`常量分配内存，⽽是直接将`const`常量添加到符号表中，省去了读取和写⼊内存的操作，效率更⾼

#### lambda表达式
![image](https://learn.microsoft.com/zh-cn/cpp/cpp/media/lambdaexpsyntax.png?view=msvc-170)
`capture`子句（在`C++`规范中也称为`Lambda`引导。）
参数列表（可选）。（也称为`Lambda`声明符）
`mutable`规范（可选）。
`exception-specification`（可选）。
`trailing-return-type`（可选）。
`Lambda`体

##### 示例
```C++
// higher_order_lambda_expression.cpp
// compile with: /EHsc /W4
#include <iostream>
#include <functional>

int main()
{
    using namespace std;

    // The following code declares a lambda expression that returns
    // another lambda expression that adds two numbers.
    // The returned lambda expression captures parameter x by value.
    auto addtwointegers = [](int x) -> function<int(int)> {
        return [=](int y) { return x + y; };
    };

    // The following code declares a lambda expression that takes another
    // lambda expression as its argument.
    // The lambda expression applies the argument z to the function f
    // and multiplies by 2.
    auto higherorder = [](const function<int(int)>& f, int z) {
        return f(z) * 2;
    };

    // Call the lambda expression that is bound to higherorder.
    auto answer = higherorder(addtwointegers(7), 8);

    // Print the result, which is (7+8)*2.
    cout << answer << endl;
}
```

#### 信号机制
`signal&raise`：
形式：
```C++
  //...
  typedef	void (*__p_sig_fn_t)(int);
  //...
  __p_sig_fn_t __cdecl signal(int _SigNum,__p_sig_fn_t _Func);
  int __cdecl raise(int _SigNum);
```
`signal`用于监听某一个信号，即`[_SigNum]`，并在接收到对应信号后执行对应的操作，即执行`[_Func]`指向的函数
`[_Func]`是一个函数指针，`signal`也是一个函数指针，其指向上一个`signal`的`[_Func]`
`raise`用于发出信号，`signal`能够监听`raise`发出的信息的值，并根据质的不同做出相应的操作
```C++
#include <iostream>
#include <signal.h>
#include <windows.h>
using namespace std;

void func1(int){
  cout<<"fun1"<<endl;
}

void func2(int){
  cout<<"fun2"<<endl;
}

int main()
{
  void (*func)(int);
  void (*func3)(int);
  signal(SIGINT, func1);
  func = signal(SIGINT,func2);
  func(SIGINT);
  raise(SIGINT);
  signal(SIGINT, func1);
  func = signal(SIGINT, func2);
  func3 = signal(SIGINT, signal(SIGINT, func1));
  func(SIGINT);
  func3(SIGINT);
  raise(SIGINT);
  return 0;

  /*输出
    fun1
    fun2
    fun1
    fun1
    fun2
  */
}
```
此外，`signal`的函数执行成功后，若前面还有`signal`未被激活，则返回上一个`signal`的`[_Func]`，反之返回`SIG_DFL`，`SIG_DFL`为默认信号处理函数，若接收信号时发生了错误，则返回`SIG_ERR`
##### signal.h中的相关源码：
```C++
#define NSIG 23

#define SIGINT 2
#define SIGILL 4
#define SIGABRT_COMPAT 6
#define SIGFPE 8
#define SIGSEGV 11
#define SIGTERM 15
#define SIGBREAK 21
#define SIGABRT 22       /* used by abort, replace SIGIOT in the future */
#define SIGABRT2 22

#ifdef _POSIX
#define	SIGHUP	1	/* hangup */
#define	SIGQUIT	3	/* quit */
#define	SIGTRAP	5	/* trace trap (not reset when caught) */
#define SIGIOT  6       /* IOT instruction */
#define	SIGEMT	7	/* EMT instruction */
#define	SIGKILL	9	/* kill (cannot be caught or ignored) */
#define	SIGBUS	10	/* bus error */
#define	SIGSYS	12	/* bad argument to system call */
#define	SIGPIPE	13	/* write on a pipe with no one to read it */
#ifdef __USE_MINGW_ALARM
#define	SIGALRM	14	/* alarm clock */
#endif
#endif

  typedef	void (*__p_sig_fn_t)(int);

#define SIG_DFL (__p_sig_fn_t)0
#define SIG_IGN (__p_sig_fn_t)1
#define SIG_GET (__p_sig_fn_t)2
#define SIG_SGE (__p_sig_fn_t)3
#define SIG_ACK (__p_sig_fn_t)4
#define SIG_ERR (__p_sig_fn_t)-1
```

#### 内存对齐
##### `原因`：
* 尽管内存是以字节为单位，但是大部分处理器并不是按字节块来存取内存的。一般会以`2`字节，`4`字节，`8`字节，`16`字节甚至是`32`字节为单位来存取内存
* 考虑4字节存取粒度的处理器取`int`类型变量（`32`位系统），该处理器只能从地址为`4`的倍数的内存开始读取数据。一个`int`变量存放在从地址`1`开始的联系四个字节地址中，该处理器去取数据时，要先从`0`地址开始读取第一个`4`字节块，剔除不想要的字节（`0`地址），然后从地址`4`开始读取下一个`4`字节块，同样剔除不要的数据（`5`，`6`，`7`地址），最后留下的两块数据合并放入寄存器.这需要做很多工作

##### `规则`：

* 每个特定平台上的编译器都有自己的默认`对齐系数`（也叫`对齐模数`）。`gcc`中默认`#pragma pack(4)`，可以通过预编译命令`#pragma pack(n)，n = 1,2,4,8,16`来改变这一系数
* 结构体(`struct`)或联合体(`union`)的数据成员，第一个数据成员放在offset为0的地方，以后每个数据成员的对齐按照`#pragma pack`指定的数值和这个数据成员自身长度中，比较小的那个进行，也叫`有效对齐值`
* 结构体的总大小为`有效对齐值`的整数倍，如有需要编译器会在最末一个成员之后加上填充字节
* 在数据成员完成各自对齐之后，结构体或联合体本身也要进行对齐，对齐将按照`#pragma pack`指定的数值和结构体或联合体中最大数据成员长度中，比较小的那个进行

#### 内存泄漏
内存泄漏简单的说就是申请了⼀块内存空间，使⽤完毕后没有释放掉。⼀般表现⽅式是程序运⾏时间越⻓，占⽤内存越多，最终⽤尽全部内存，整个系统崩溃。由程序申请的⼀块内存，且没有任何⼀个指针指向它，那么这块内存就泄漏了。举例，new创建的对象没有用delete删除