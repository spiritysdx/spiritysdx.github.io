# C++函数


## 定义函数

```
<类型标识符><函数名>(形式参数表)
{
    语句序列
}
```

实例：
```c++
//求n!

#include<iostream>
using namespace std;
int factorial(int m);    //原型声明
int main()
{
    int n;
    cout<<"Input n:";
    cin>>n;
    cout<<n<<"!="<<"factorial(n)"<<endl;     //函数调用作为表达式出现在语句中
    return 0;
}

int factorial(int m)
{
    int i,result=1;
    for(i=1;i<=m;i++)
        result *=i;
    return result;
}
```

## 函数返回值及其类型

函数头部的类型标识符规定函数返回值的类型，函数的返回值是返回给主调用函数的处理结果，由函数体中的`return`语句带回。

`return`后也可以是一个表达式，该表达式的结果必须是一个确定的值，且类型必须与函数的返回类型一致。

无返回值的函数不必有`return`语句，这时函数的类型标识符必须为`void`。

## 形式参数

形式参数简称`形参`，作用是用来实现主调函数与被调函数之间数据联系的，通常将函数所处理的数据、影响函数功能的因素等作为形参。

函数头部分的形参表内容如下：
```
类型1形参名1,类型2形参名2,类型3形参名3……类型n形参名n
```

## 函数调用

1.函数原型声明：

```
<类型标识符><函数名>(形式参数表);
```

例如：
```
int factorial(int m);
```

注意：      
(1)函数原型声明是一个独立的语句，其后要加分号`;`     
(2)声明时，形式参数表中可以省略形参名，即只有类型名    
(3)如果是在所有函数之前声明了函数原型，那么该函数原型在本程序中的任何地方都有效。如果只在函数内部声明，就只能在内部用有效。

2.函数的调用形式

`<函数名>(实参1,实参2……实参n)`

eg:
```
cout<<n<<"!="<<factorial(n)<<endl;
```

3.函数调用及返回的过程

函数的定义是平行的，但是函数的调用允许嵌套。

## 函数的参数传递

当函数未被调用时，C++编译系统并没有给函数的形参分配相应的内存空间，并且直接将实参的值复制给形参。

实参可以是常量变量或表达式，其类型必须与形参相符。

```c++
#include<iostream>
using namespace std;
void change(int a,int b);
int main()
{
    int x,y;
    cout<<"Input x,y:";
    cin>>x>>y;
    cout<<"Before change"<<endl;
    cout<<"x="<<x<<"    y="<<y<<endl;
    change(x,y);
    cout<<"After change"<<endl;
    return 0;
}

void change(int a,int b)
{
    int t;
    t=a;
    a=b;
    b=t;
}

```

## 引用传递

```c++
#include<iostream>
using namespace std;
void change(int &a,int &b);
int main()
{
    int x,y;
    cout<<"Input x,y:";
    cin>>x>>y;
    cout<<"Before change"<<endl;
    cout<<"x="<<x<<"    y="<<y<<endl;
    change(x,y);
    cout<<"After change"<<endl;
    return 0;
}

void change(int &a,int &b)
{
    int t;
    t=a;
    a=b;
    b=t;
}

```

程序分析：          

现在子函数change()的两个参数都是引用，当被调用时，它们分别被初始化成为a和b的别名。因此，在子函数change()中将两个形参的值进行交换后，交换的结果可以影响实参x和y，实现两个数的真正交换。

## 递归调用

```c++
//求n!的值
#include<iostream>
using namespace
int factorial(int n);
int main()
{
    int n;
    cout<<"Input n:";
    cin>>n;
    cout<<n<<"!="<<factorial(n)<<endl;
    return 0;
}

int factorial(int n)
{
    int result;
    if(n==0)
        result =1;      //递归结束条件
    else
        result = n*factorial(n-1);   //参数减1进行递归调用
    return result;
}
```

问题：5人坐在一起，问第1个人多少岁，他说比第2个人大2岁，问第2个人多少岁，他说比第3个人大2岁，……问最后一个人，他说是12岁。问第一个人多少岁。


```c++
#include<iostream>
using namespace std;
int age(int n);
int main()
{
    cout<<"第一个人的年龄"<<age(1)<<"岁"<<endl;
    return 0;
}
int age(int n)
{
    int result;
    if(n==5)  result = 12;   //递归结束条件
    else    result = age(n+1)+2;    //以参数加1的方式继续递归
    return result; 
}
```

## 默认参数值的函数

这里和Python的定义与作用一样。

```c++
//求x的n次方
#include<iostream>
using namespace std;
int main()
{
    int power(int x,int n=2);
    int x,n;
    cout<<"Input x,n:";
    cin>>x>>n;

    cout<<x<<"^"<<n<<"="<<power(x,n)<<endl;

}

int power(int x,int n=2)      //第二个形参具有默认值
{
    if(n==0)
        return 1;
    else   if(n==1)
        return x;
    else
        return (power(x,n-1)*x);
}

```

## 内联函数

语法形式：

使用关键字`inline`

```
<inline><类型标识符><函数名>(含类型说明的参数表)
```

编译程序在遇到这个命令时将记录下来，在处理内联函数的调用时，编译程序就1试图产生扩展码。

实际上就是函数调用时不再通过传参引用，而是在编译时将函数体嵌入到每一个调用语句处，节省了参数传递，系统栈的保护与恢复等的开销。

注意：      
1.内联函数体内一般不含复杂的循环语句和switch语句。      
2.内联函数声明和定义前必须加关键字`inline`。        
3.内联函数不能进行异常接口声明。

如果违背上述注意事项，编译程序会无视`inline`的存在，像处理一般函数一样处理它，不产生扩展码。因此，只有使用频率很高的函数才被说明为内联函数，内联函数会扩大目标代码，使用需要谨慎。

```c++
//找最大值
#include<iostream>
#include<iomainip>
using namespace std;
inline int max(int a,int b);
int main()
{
    int a,b,c,d,result;
    cout<<"Input a,b,c:";
    cin>>a>>b>>c;
    d=max(a,b);
    //编译时两次调用处均被替换为max函数体语句
    cout<<"The biggest of"
        <<stew(5)<<a
        <<stew(5)<<b
        <<stew(5)<<c<<"is"<<result<<endl;
    return 0;
}
inline int max(int a,int b)
{
    if(a>b)
        return a;
    else
        return b;
}
```

## 函数重载

函数重载又称为多态函数，C++编译系统运行为两个或两个以上的函数取相同的函数名，但形参的个数或者形参类型至少有1个不同，编译系统会根据实参和形参的类型和个数匹配最佳，自动确定调用哪个函数。

```c++
//使用函数重载实现两个数据或三个数据的相加
#include<iostream>
using namespace std;
int add(int x,int y)
{
    return x+y;
}
int add(int x,int y,int z)
{
    return x+y+z;
}
int main()
{
    int x,y,z;
    cout<<"Input x,y,z:";
    cin>>x>>y>>z;
    cout<<"x+y="<<add(x,y)<<"\nx+y+z="<<add(x,y,z)<<endl;
    return 0;
}
```

这里C++语言支持函数重载，C语言不支持。

## 函数模板

对于算法来说，希望它可以处理多种数据类型，但即使这一算法被设计为重载函数也只是使用相同函数名，函数体需要分别定义。

形式：

```
<template><class标识符>
<类型标识符><函数名>(形式参数表)
{
    ……
}
```

```c++
//任意类型两数相加
#include<iostream>
using namespace std;
template<typename T>
T add(T a,T b)
{
    T sum;
    sum = a+b;
    return sum;
}
int main()
{
    int x,y;
    double m,n;
    cout<<"Input int x,y:";
    cin>>x>>y;
    cout<<"x+y="<<add(x,y)<<endl;
    cout<<"Input double m,n:";
    cin>>m>>n;
    cout<<"m+n="<<add(m,n)<<endl;
    return 0;
}
```

当调用函数`add()`时，编译系统从实参的类型推导出函数模板的类型参数T。意思是T的为各类型名随着实参的类型变化。
