# C++简单程序

## 赋值表达式

常规类似python，同样有赋值运算的简写形式，如

```c++
b += 2          //b = b + 2
x *= y + 3      //x = x*(y+3)
x += x -= x*x   //x = x + (x = x - x*x) 
```

## 逗号表达式

在C++中，逗号也是一个运算符，使用形式为

<表达式1>,<表达式2>,……,<表达式n>

上述表达式的求解顺序为先求解表达式1，再求解表达式2，……，逗号表达式的最终结果为表达式n的值。

```c++
#include <iostream>
using namespace std;
int main( )
{
    //赋值
    int a=1,c=2,n,m;
    n = 2-(a=3);
//    n=2-a = 3     //错误
    n += m -= a*a;
    cout<<"c+2="<<c+2<<endl;
//  cout<<n = m = a<<endl;   //错误

    a = (c=5,c+5,c/2);
    cout<<"a="<<a<<endl;     //a=2,注意c+5并没给c赋值
// cout<<a=(c=5,c+5,c/5)<<endl;     //错误
// cout<<a-c+5<<endl;

    int i=12;
    i + = i  =i * =i;
    cout<<"i="<<i<<endl;
    n = m = a;
    cout<<"n="<<n<<",="<<m<<endl;
    return 0;
}
```


程序运行情况: 

```C++
C+2=4
a=2
i=0
n=2,m=2
```

## 关系表达式&比较运算符

记住下面这段东西：

```c++
// 优先级相同(较高)
<,<=,>,>=

//优先级相同(较低)
==,!=

//以上运算符与Python一样
```

注意：==是两个等于号，表示判断左边是否等于右边，返回值是true或者false。这里与Python中的判断语句一样。

## 逻辑表达式

!(非)      &&(与)     ||(或)

优先级为前高后低

!是一元运算符，作用是操作数取反

&&和||是二元运算符，作用是判断返回值为true或false。

## 条件表达式

条件运算符`?:`是C++中唯一的三元运算符，作用是实现简单的选择功能，形式如下：

```
<测试表达式>?<表达式1>:<表达式2>
```

其中，测试表达式通常是bool类型，表达式1和表达式2可以是任何类型。

执行顺序：    
1.求解测试表达式，求解出值;    
2.若值为true，执行表达式1，表达式1的结果就是该条件表达式结果;    
3.若值为false，执行表达式2，表达式2的结果就是该条件表达式结果;     

注意：    
条件运算符的优先级高于赋值运算符，低于逻辑运算符，结合方向自右向左。   

eg:将a与b中的最大值赋值给max

```c++
max=(a>b)?a:b;
```

## sizeof运算符

计算某种数据类型在内存中所占字节数。

```c++
//XXX是类型名
sizeof(XXX)
//或者XXX是表达式
sizeof(XXX)
//注意，这里的表达式并不会求解，只会返回表达式结果的类型所占的字节数
```

## 位运算

这是Python中所没有的，高级语言处理数据最小单位只能是字节，C++可以对数据按二进制位进行操作。

C++中有6个位运算符，只能对整型数据进行操作。

1.按位与(&)

将两个操作数对应每一位分别进行逻辑与操作。

```
3       00000011
5(&)    00000101
------------------
3&5     00000001
```

这里与0相与，清零该位；与1相与，维持不变。

最常用的使用方法是将操作数中的若干位清零，或者取操作数中的若干指定位。

(1)将char型变量a的最低位清零：

```c++
a=a&0xfe
```

(2)设变量c是char型变量，变量a是int变量，下列语句可取出a的低字节，并放置于c中：

```c++
c=a&0337;
```

2.按位或(|)

将两个操作数的对应每一位分别进行逻辑或操作。

```
3       00000011
5(|)    00000101
------------------
3|5     00000111
```

常用于将操作数中若嘎巴位的值置为1(其他位保持不变)。

将int型变量a的低字节置为1：

```c++
a=a|0xff
```

3.按位异或(^)

将两个操作数对应每一位进行异或操作(对应位相同为0，不同为1)。

```
071         00111001
051(^)      00101010
------------------------
071^051     00010011
```

常用于将操作数中的若干指定位取反。

与0异或，结果是该位原值；与1异或，结果是该位值取反。

4.按位取反(~)

按位取反是一个单目运算符，作用是对一个二进制数的每一位取反。

```
025         00010101
~025        11101010
```

5.移位

C++中有两个移位运算符：左移位运算符(<<)，右移位运算符(>>)。它们都是二元运算符。

移位运算符左边是操作数，右边是需要移动的位数。

移位规则：     
左移，低位补0，移出的高位舍弃。      
右移，低位舍弃，无符号数高位补0，有符号数高位补符号位。   

```
设 short a = 65 ，则a>>1的操作过程如下              
移位前：   0  0 0 0 0 0 0 0 0 1 0 0 0 0 0 1        
移位后：  (0) 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 (1)    
        补1位0                            舍弃低1位 
```

```
设 2<<4的值为32 ，则2<<4的操作过程如下              
移位前：            0 0 0 0 0 0 0 0 0 0 0  0 0 0 1        
移位后：  (0 0 0 0) 0 0 0 0 0 0 0 0 0 0 1 (0 0 0 0)    
          舍弃高4位                         补4位0 
```

注意：移位运算的结果是位运算表达式的值。移位后，左边的值不变，即a>>1后，a还是原值65。

## 数据类型转换

1.将一种类型值赋值给另一种类型的变量

```c++
#include<iostream>
using namespace std;
int main()
{
    //整型和实型的赋值
    int i = 3.56;              //i赋值为3
    int j = 1.8E12;            //数值超出类型取值范围，j不能被正确赋值
    float n = 5;               //n被赋值为5.0
    cout<<" int i = "<<i<<endl;
    cout<<" int j = "<<j<<endl;
    cout<<" float n = "<<n<<" , n/2 ="<<n/2<<endl;

    //bool类型的赋值
    bool t1 = 19,t2 = 0;       //t1被赋值为true，t2被赋值为false
    cout<<" bool t1 = "<<t1<<" , bool t2 = "<<t2<<endl;

    //取值范围不同类型的赋值
    short s1 = 50;
    long l1 = s1;
    cout<<" short s1 = "<<s1<<" , long l1 ="<<l1<<endl;
    long l2 = 3500;
    short s2 = l2;             //数值超过类型取值范围，s2不能被正确赋值
    cout<<" long l2 = "<<l2<<" , short s2 ="<<s2<<endl;
    double d = 3.4E50;
    float f = d;               //数值超出类型取值范围，f不能正确赋值
    cout<<" double d = "<<d<<" , float f ="<<f<<endl;
    return 0
}
```

程序运行结果：

```
int i = 3
int j = 408702976
float n = 5,n/2 = 2.5
bool t1 = 1,bool t2 = 0
short s1 = 50,long l1 = 50
long l2 = 3500,short s2 = -30536
double d = 3.4e+050,float = 1.#INF
```

小范围值可以赋值给大范围值，大范围值无法赋值给小范围值，此时会显示#INF表示不能正确赋值。

赋值运算要求左值与右值的类型相同，不同会自动进行类型转换，其转换的规则是将右值类型转换为左值的类型。

bool值赋值时，非0则赋值为true，0赋值为false。

## 表达式的隐含转换

混合运算中二元运算符要求两边类型一致，若不一致，自动进行转换，由低到高。

大小关系如下

```
char    short    int    unsigned    long     unsigned-long     float      double
---------------------------------------------------------------------------------
低                                                                          高
```

## 强制类型转换&显式转换

```
<类型标识符>(表达式)
或
(类型标识符)<表达式>
```

这里类似Python。

## 输入与输出

1.cout

```c++
cout<<"\"This is a simple.\",he said.\n";
```

输出结果为：

"This is a simple.",he said.

2.cin

程序需要键盘输入时，用提取操作符(>>)从cin输入流中抽取字符，格式如下

```
cin>>变量名>>……
```

cin是预定义的流类对象，>>是预定义的提取符，cin能自动识别输入的数据类型。

## I/O流控制符

包含在iomanip中

```c++
#include<iomanip>
```

头文件引入后可使用dec,hex,oct,ws,endl,ends,setprecision(int),setw(int)

其中较为常用的是设置宽域的setw()，插入换行符并刷新流的endl，插入空字符的ends。

```c++
cout<<setw(8)<<10<<20<<endl;
```

运行结果为
```
------1020
```

例子：

```c++
//用格式控制符控制输出
#include<iostream>
#include<iomainip>
using namespace std;
int main()
{
    int k;
    double n;
    cout<<"输入十进制整数K和实数n的值：";
    cin<<k<<n;

    cout<<"\tDecimal:"<<dec<<k<<endl
        <<"\tHexadecimal:"<<hex<<k<<endl
        <<"\t0ctal:"<<oct<<k<<endl;
    
    cout<<"设置域宽setw(5):\n"<<setw(5)<<n<<endl;    //5<7
    cout<<"设置域宽setw(10):\n"<<setw(10)<<n<<endl;    //10>7
    cout<<"设置小数位数setprecision(4):\n"<<setprecision(4)<<n<<endl;
    return 0
}
```

程序运行情况：
```
输入十进制整数K和实数n的值：1001               3.14159
Decimal:1001
Hexadecimal:3e9
Octal:1751
设置域宽setw(5):
3.14159
设置域宽setw(10):
    3.14159
设置小数位数setprecision(4):
2.142
```

## 插入部分C语言控制符和函数

1.printf("格式控制符",输出列表)和scanf("格式控制符",地址列表)      
```c++
#include<iostream>
using namespace std;
int main()
{
    int k;
    float n;
    char ch;
    printf("Input k,n,ch")
    scanf("%d%f,%c",&k,&n,&ch);
    printf("k=%d,n=%f,ch=%c\n",k,n,ch);
    return 0
}
```

程序运行情况：
```
Input k,n,ch:5 3.14,a
k=5,n=3,ch=a
```

```printf("x=%dy=%f\n",x,y);```    
等同于    
```cout<<"x="<<x<<"y="<<y;```


2.getchar()和putchar()

```c++
#include<iostream>
using namespace std;
int main()
{
    char ch;
    printf("Input ch:");

    //scanf()和printf()
    scanf("%c",&ch);
    printf("ch=5c\n",ch)

    //getchar()和putchar()
//  ch=getchar();
//  putchar(ch);
//  putchar('\n');

    return 0;
}
```

程序运行情况：

```
Input ch:A
ch=A
```

## 数据溢出

```c++
#include<iostream>
using namespace std;
int main()
{
    short a=32767,b;
    cout<<"Input b:";
    cin>>b;
    a = a+b;
    cout<<"sizeof="<<sizeof(short)<<endl;
    cout<<"a="<<a<<",b="<<b<<endl;
    return 0;
}
```

测试用例：

```
Input b:2
sizeof=2
a=-32767,b=2
```





