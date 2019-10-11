---
title: "通过函数形参改变传入指针的值"
date: 2019-10-11
categories:
  - 计算机编程
tags:
  - c++
slug: func_pointer
---

例子：
```c++
#include <iostream> 
void foo(char *p)
{
   p = "after foo()";
}     
void main()
{
   char *p = "before foo()";
   foo(p);     
   cout <<p <<endl;   
}
```
如果你指望函数foo能帮你改变p的值，那你就错了。因为指针也是变量，它在函数调用过程中也是传值调用的（C中函数参数都是传值调用）。函数foo中的p只是局部变量，它的作用域仅在子函数中。上面这段代码其实和下面这段效果是类似的：
```c++
#include <iostream> 
void foo(int i)
{
   i = 1;
}     
void main()
{
   int i = 0;
   foo(i);     
   cout <<i<<endl;   
}
```

这是foo函数显然不会修改i的值，对于第一段代码的指针，也是同样的道理。都是变量的传值调用，一个是整形变量，一个是指针变量而已。

那么，如何修改代码才能得到我们想要的效果呢？有三种方法。

一、使用return

```c++
#include <iostream>
char* foo()
{
 char* p = "after foo()";
 return p;
}
void main()
{
 char* p = "before foo()";
 p = foo();
 cout<<p<<endl;
}
```

二、使用指针引用

```c++
#include <iostream> 
void foo(char *& q)
{
   q = "after foo()";
}     
void main()
{
   char *p = "before foo()";
   foo(p);     
   cout <<p <<endl;   
}
```

该程序将p的地址传给foo，则p=&q，而p是字符串变量的内存地址，那么&q也是字符串变量的内存地址，则q也是指向该字符串变量所在内存的指针。

简单的说foo中的q这时是p的引用（别名），q和p共享同一个内存空间。这时在foo中修改q指向的内存空间的字符串内容，main中p的值当然也随之变化。

三、使用指向指针的指针

```c++
#include <iostream> 
void foo(char ** p)
{
   *p = "after foo()";
}     
void main()
{ 
   char **p = "before foo()";   
   foo(p);     
   cout <<*p<<endl;   
}
```

main中p是指向指针的指针，即它的值的值是指向字符串变量内存的指针的地址，在foo中p就表示该指针，即字符串变量内存的地址。所以修改p的值自然可以修改该内存的内容。

下面一段程序使用了同样的原理：

```c++
#include <iostream> 
void foo(char ** p)
{
   *p = "after foo()";
}     
void main()
{ 
   char *p = "before foo()";   
   foo(&p);     
   cout <<p<<endl;   
}
```

[参考1：改变函数形参传入指针的值的方法](https://blog.csdn.net/jack237/article/details/7355995)

[参考2：改变函数形参传入指针的值的方法](https://blog.csdn.net/qiqi__xu/article/details/89672005)

[参考3：通过指针形参修改实参的值1(图解)](https://www.cnblogs.com/acgpiano/p/4017858.html)

[参考4：通过指针形参修改实参的值2(图解)](https://www.cnblogs.com/acgpiano/p/4017964.html)