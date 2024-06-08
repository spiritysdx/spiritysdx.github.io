# C++数值逼近-Euler方法、改进Euler方法和经典四阶Runge-Kutta方法求解初值问题


# 数值积分

取步长 h = 0.1 ，分别用 Euler 方法、改进 Euler 方法和经典四阶 Runge-Kutta 方法求解初值问题
```
f(x,y) =  ((y)-2*(x)/(y))
f(0)  =   0
```
并将计算结果与```sqrt(1+2*(x))```精确解相比较。

```c++
#include<stdio.h>
#include<math.h>
#include<iostream>
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
using namespace std;

#define f(x,y)  ((y)-2*(x)/(y))
#define y(x)    sqrt(1+2*(x))

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);

    float a = 0,b = 1.0f,h=0.1f,y0=1.0f,x,ye,yp,ym,k1,k2,k3,k4,yr,yx;
    printf("\n    精确解            Euler方法");
    printf("          改进Euler方法     4阶Runge-Kutta方法");
    printf("\n x       y      ye[k]    |ye[k]-y|   ");
    printf("ym[k]   |ym[k]-y|   yr[k]   |yr[k]-y|\n");
    printf("%3.1f  %8.6f  %8.6f  %8.6f  ",a,y0,y0,0);
    printf("%8.6f  %8.6f  %8.6f  %8.6f  \n",y0,0,y0,0.0);

    x=a;
    ye=y0;     //Euler方法的初值
    ym=y0;     //改进的Euler方法的初值
    yr=y0;     //4阶Runge-Kutta方法的初值

    while(x<b)
    {
        ye = ye + h*f(x,ye);   

        yp = ym + h*f(x,ym);

        ym = ym + h/2*(f(x,ym)+f(x+h,yp));

        k1 = f(x,yr);
        k2 = f(x+h/2,yr+h/2*k1);
        k3 = f(x+h/2,yr+h/2*k2);
        k4 = f(x+h,yr+h*k3);
        yr = yr + h/6*(k1+2*k2+2*k3+k4);

        x = x + h;
        yx = y(x);
        printf("%3.1f  %8.6f  %8.6f  %8.6f  %8.6f  %8.6f  %8.6f  %8.6f\n",x,yx,ye,fabs(ye-yx),ym,fabs(ym-yx),yr,fabs(yr-yx));
    }
    system("pause");
    return 0;
}
```
