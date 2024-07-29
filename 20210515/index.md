# C++数值逼近-迭代法Aikten法以及牛顿法求解线性方程根通用程序

# 数值逼近 

公元 1225 年，比萨的数学家 Leonardo（即 Fibonacci（斐波那契）），1170-1250）研究了方程

```x^3+2*x^2+10*x-20=0```

得到一个根 x* = 1.368 808 107，没有人知道他用什么方法得到这个值。对于这个方程，分别用下列方法： 
（1）迭代法式1 ； 
（2）迭代法式2 ；
（3）对（1）的 Aitken 加速方法； 
（4）对（2）的 Aitken 加速方法； 
（5）Newton 法 。
求方程的根（可取 x0 = 1 ），计算到 Leonardo 所得到的准确度。


```c++
#include<stdio.h>
#include<math.h>
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
#include<iostream>
using namespace std;

#define phi01(x)   20/((x)*(x)+2*(x)+10)//迭代函数

#define phi02(x) (20-2*((x)*(x))-((x)*(x)*(x)))/10

#define f(x)   ((x)*(x)*(x))+2*((x)*(x))+10*(x)-20

#define f1(x)  3*((x)*(x))+4*(x)+10

void test01()
{
    int n=1,N;
    double x,x0,del;
    printf("x0 = 1");
    //scanf("%f",&x0);
    x0 = 1;
    printf("\ndel = 0.000000001");
    //scanf("%f",&del);
    del = 0.000000001;
    printf("\nN = 100");
    //scanf("%d",&N);
    N = 100;
    printf("\n k     x(k)");
    printf("\n  %2d  %f",0,x0);
    while (n<N)
    {
        x = phi01(x0);
        if(fabs(x-x0)<del)
        {
            printf("\n\n 迭代法式1的近似解 = %.9lf \n",x);
            return;
        }
        printf("\n %2d   %.9lf",n,x0);
        n = n+1;
        x0 = x;
    }
    printf("\n\n%d次迭代后未达到精度要求.\n",N);
}

void test02()
{
    int n=1,N;
    double x,x0,del;
    printf("x0 = 1");
    //scanf("%f",&x0);
    x0 = 1;
    printf("\ndel = 0.000000001");
    //scanf("%f",&del);
    del = 0.000000001;
    printf("\nN = 20");
    //scanf("%d",&N);
    N = 20;
    printf("\n k     x(k)");
    printf("\n  %2d  %f",0,x0);
    while (n<N)
    {
        x = phi02(x0);
        if(fabs(x-x0)<del)
        {
            printf("\n\n 迭代法式2的近似解 = %.9lf \n",x);
            return;
        }
        printf("\n %2d   %.9lf",n,x0);
        n = n+1;
        x0 = x;
    }
    printf("\n\n%d次迭代后未达到精度要求.\n",N);
}

void Aitken01()
{
    int n=1,N;
    double x0,x1,x2,x3,del;
    printf("x0 = 1");
    //scanf("%f",&x0);
    x0 = 1;
    printf("\ndel = 0.000000001");
    //scanf("%f",&del);
    del = 0.000000001;
    printf("\nN = 100");
    //scanf("%d",&N);
    N = 100;
    printf("\n k     x(k)");
    printf("\n  %2d  %f",0,x0);
    while (n<N)
    {
        x1 = phi01(x0);
        x2 = phi01(x1);
        x3 = x2-(((x2-x1)*(x2-x1))/(x2-2*x1+x0));
        if(fabs(x3-x0)<del)
        {
            printf("\n\n 迭代法式1的Aitken近似解 = %.9lf \n",x3);
            return;
        }
        printf("\n n =   %2d  x1 =  %.9lf",n,x3);
        n = n+1;
        x0 = x3;
    }
    printf("\n\n%d次迭代后未达到精度要求.\n",N);
}

void Aitken02()
{
    int n=1,N;
    double x0,x1,x2,x3,del;
    printf("x0 = 1");
    //scanf("%f",&x0);
    x0 = 1;
    printf("\ndel = 0.000000001");
    //scanf("%f",&del);
    del = 0.000000001;
    printf("\nN = 100");
    //scanf("%d",&N);
    N = 100;
    printf("\n k     x(k)");
    printf("\n  %2d  %f",0,x0);
    while (n<N)
    {
        x1 = phi02(x0);
        x2 = phi02(x1);
        x3 = x2-(((x2-x1)*(x2-x1))/(x2-2*x1+x0));
        if(fabs(x3-x0)<del)
        {
            printf("\n\n 迭代法式2的Aitken近似解 = %.9lf \n",x3);
            return;
        }
        printf("\n n =   %2d  x1 =  %.9lf",n,x3);
        n = n+1;
        x0 = x3;
    }
    printf("\n\n%d次迭代后未达到精度要求.\n",N);
}

void Newton()
{
    int n=1,N;
    double x,x0,del;
    printf("x0 = 1");
    //scanf("%f",&x0);
    x0 = 1;
    printf("\ndel = 0.000000001");
    //scanf("%f",&del);
    del = 0.000000001;
    printf("\nN = 100");
    //scanf("%d",&N);
    N = 100;
    printf("\n k     x(k)");
    printf("\n  %2d  %f",0,x0);
    while (n<N)
    {
        float f_1 = f(x0);
        float f_2 = f1(x0);
        float d = f_1/f_2;
        x =x0 - d;
        if(fabs(x-x0)<del)
        {
            printf("\n\n Newton迭代法的近似解 = %.9lf \n",x);
            return;
        }
        printf("\n %2d   %.9lf",n,x);
        n = n+1;
        x0 = x;
    }
    printf("\n\n%d次迭代后未达到精度要求.\n",N);
}

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);

    test01();
    cout<<endl;
    
    test02();
    cout<<endl;

    Aitken01();
    cout<<endl;

    Aitken02();
    cout<<endl;

    Newton();
    cout<<endl;

    system("pause");
    return 0;
}
```
