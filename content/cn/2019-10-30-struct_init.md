---
title: "struct结构体如何初始化(出现does not name a type错误)"
date: 2019-10-30
categories:
  - 计算机编程
tags:
  - c++
slug: struct_init
---

有时给struct赋值时会出现`does not name a type`的错误，如下：
```c++
struct MyStruct{
    int x;
};

MyStruct structVar;

structVar.x = 1;

int main(){
    return;
}
```

问题在于在main函数外只能创造变量和初始化，不能进行赋值操作。但可以在main函数内进行赋值。解决方法一般有两种：

第一种是在main函数外，进行初始化
```c++
struct MyStruct{
    int x;
};

MyStruct structVar = {1};

int main(){
    return;
}
```

第二种是在main函数内，可进行赋值
```c++
struct MyStruct{
    int x;
};

int main(){
    MyStruct structVar;
    structVar.x = 1;
    return;
}
```

常见的结构体初始化方法：

结构体
```c++
struct MyStruct{
    int x;
    int y;
};
```

初始化1

```c++
struct MyStruct structVar={
    .x=1,
    .y=2
};
```

初始化2

```c++
struct MyStruct structVar={
    x:1,
    y:2
};
```

初始化3

```c++
struct MyStruct structVar={1,2};
```

Linux内核喜欢用第一种，使用第一种和第二种时，成员初始化顺序可变。

初始化4 

因为C++中的struct可以看作class，结构体也可以拥有构造函数，所以我们可以通过结构体的构造函数来初始化结构体对象。

给定带有构造函数的结构体：

```c++
struct MyStruct{
    MyStruct(int x,int y){
        this->x=x;
        this->y=y;
    };
    int x;
    int y;
};
```

```c++
struct MyStruct structVar(1,2);
```
注意： struct如果定义了构造函数的话，就不能用大括号进行初始化了，即不能再使用前三种初始化的方式了。


变量的赋值和初始化是不一样的，初始化是在变量定义的时候完成的，是属于变量定义的一部分，赋值是在变量定义完成之后想改变变量值的时候所采取的操作。

还是给定结构体
```c++
struct MyStruct{
    int x;
    int y;
};
```

注意：结构体变量的赋值是不能采用大括号的方式进行赋值的，例如下面的赋值是不允许的:

```c++
struct MyStruct structVar;

structVar={1,2};//错误
```

下面列出常见结构体变量赋值的方法。

赋值1

使用memset对结构体变量进行置空操作：
```c++
//按照编译器默认的方式进行初始化（如果structVar是全局静态存储区的变量，默认初始化为0，如果是栈上的局部变量，默认初始化为随机值）
struct MyStruct structVar; 
memset(&structVar,0,sizeof(structVar));
```

赋值2

依次给每一个结构体成员变量进行赋值：
```c++
struct MyStruct structVar; 
structVar.x=1;
structVar.y=2;
```

赋值3

使用已有的结构体变量给另一个结构体变量赋值。也就是说结构体变量之间是可以相互赋值的。

```c++
struct MyStruct structVar1={1,2};
struct MyStruct structVar2;
structVar2=structVar1;               //将已有的结构体变量付给tructVar2
```