# C++入门

如果有Python的基础，学习c++会感觉很别扭2333       
如果是用VS写c++程序，注意打开文件夹包含.vscode文件夹，否则会运行找不到路径。      
(Python就没那么麻烦，不用整json配置)     

## 注释

```c++
/* 这是C++的
多行注释方法*/
/*******************/

//这是c++的单行注释方法
```

注释约定俗成写在程序上方或右方

## 头文件

```c++
#include<iostream>
```

.cpp文件开头在编译前将`iostream.h`文件中的代码嵌入到程序中，作为程序的一部分。

## 主函数

```c++
int main()
{
    cout<<"hellow!"<<endl;
    cout<<"I am student"<<endl;
    return 0;
}
```

`main()`之前的`int`表示主函数返回值的类型是整数类型`int`,函数主体用了花括号`{}`括起来，每个语句由`;`作为结束符。

另外，C++程序都是由函数构成的，在C++程序中，有且只能有一个`main()`函数，c++程序从`主函数main()`开始执行。

`cout`是输出语句，`return`是函数返回语句。

## 名称空间

```c++
using namespace std;
```

这里的`std`是名称空间，类似python在库中预定义的方法一样，避免自己使用的名称存在歧义(即自己命名的标识符和所使用的类库的标识符重合的情况)，python中通过前缀库名解决，c++通过开头声明`名称空间`来解决。

## 第一个c++程序

```c++
/*第一个c++程序*/
#include<iostream>
using namespace std;
int main()
{
    cout<<"Hello world!"<<endl;
    return 0;
}
```

## 其他注意事项

1.编程预处理语句：`#`开头的行，      
2.一个程序函数名可以很多，但是每个程序必须包含`main()`，程序总是从main()开始执行，不管`main()`处于程序哪个位置，      
3.程序中符号都是英文符号，不是中文符号，      
4.函数体由`{}`括起来，一般包含变量定义和程序功能实现部分，所有变量需要`先定义再使用`，    
5.程序中标识符分大小写，      
6.凡是空格符出现的地方都可以换行表示，运行结果一样。

## 数据类型

测试数据类型字节数

```c++
#include <iostream>
using namespace std;

int main()
{
   cout << "Size of char : " << sizeof(char) << endl;
   cout << "Size of int : " << sizeof(int) << endl;
   cout << "Size of short int : " << sizeof(short int) << endl;
   cout << "Size of long int : " << sizeof(long int) << endl;
   cout << "Size of float : " << sizeof(float) << endl;
   cout << "Size of double : " << sizeof(double) << endl;
   cout << "Size of wchar_t : " << sizeof(wchar_t) << endl;
   return 0;
}
```

## 转义字符`\`

\n     换行    
\t     横向跳格，输出位置跳到下一个制表位     
\b     退格，输出位置回退一个字符   
\r     回车，输出位置回退到本行开头   
\a     响铃       

这里的`\`类似Python中的`r`，作为转义字符使用   
`\\`   两个反斜杠实际表示一个反斜杠   
\'     表示'   
\"     表示"   
\ddd   表示1~3位的八进制数   
\xhh   表示1~2位的十六进制数   



```c++
#include<iostream>
using namespace std;
int main()
{
    //整型常量
    int a=3;
    int b=023;
    int c=0x3a;
    int d=0x3A;
    cout<<"整型常量：\n赋值\t\t输出\n";
    cout<<"a=3,\t\ta="<<a<<endl;
    cout<<"b=023,\t\tb="<<b<<endl;
    cout<<"c=0x3a,\tc="<<c<<endl;
    cout<<"\144=0x3A,\t\x64="<<d<<endl;

    //实型常量
    double e=30000;          //一般形式，大数
    double f=0.00012;        //一般形式，小数
    double g=3.0E+4;         //指数形式，大数
    double h=0.12e-3;        //指数形式，小数
    cout<<"\xA实型常量：\12赋值\t\t输出\x0a";
    cout<<"e=30000.0,\te="<<e<<endl;
    cout<<"f=0.0012\tf="<<f<<endl;
    cout<<"g=3.0E+4,\tg="<<g<<endl;
    cout<<"h=0.12e-3,\th="<<h<<endl;
}
```

## 变量声明与引用

声明形式如下

`<类型> 变量名1,变量名2,……;`

引用形式如下

`<类型> &引用名=目标名;`

eg：

```c++
int r=10;   //声明
int &qr=r;  //引用
```

实际qr=r=10,这里就是引用。   
(这里如果是Python则直接用等号即可，c++需要在引用名前添加&)

## 运算符

常规类似python，不同的如下。

1.单独使用自增自减运算符

```c++
int i=1,j=1;

//前后置自增运算符单独执行
cout<<"i="<<i<<endl;
cout<<"j="<<j<<endl;
i++;++j;
cout<<"i="<<i<<endl;
cout<<"j="<<j<<endl;
//运算后结果两者都为2
```

2.参与其他操作的自增自减运算符
```c++
int i=1,j=1;

//参与其他操作的自增运算符
cout<<"i="<<i<<endl;
cout<<"j="<<j<<endl;
cout<<"i++="<<i++<<endl;
cout<<"++j="<<++j<<endl;
/*
i=1
j=1
i++=1
++j=2
*/
```

注意：自增自减运算符后置优先于前置。
