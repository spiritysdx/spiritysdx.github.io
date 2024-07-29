# C++数值逼近-迭代法求非线性方程的根


# 数值逼近

用迭代法求非线性方程```(x+1)^2-sinx-3=0```的根。
取迭代函数```0.5*(3+sin((x))-1-(x)*(x))```，精度要求为```1*10^-2```，最多迭代 100 次。

```c++
#include<stdio.h>
#include<math.h>
#define phi(x)   0.5*(3+sin((x))-1-(x)*(x))    //迭代函数
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
#include<iostream>
using namespace std;

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);
    
    int n=1,N;
    float x,x0,del;
    printf("x0 = ");
    scanf("%f",&x0);
    printf("\ndel = ");
    scanf("%f",&del);
    printf("\nN = ");
    scanf("%d",&N);
    printf("\n k     x(k)");
    printf("\n  %2d  %f",0,x0);
    while (n<N)
    {
        x = phi(x0);
        if(fabs(x-x0)<del)
        {
            printf("\n\n 近似解 = %f \n",x);
            return 0;
        }
        printf("\n %2d   %f",n,x0);
        n = n+1;
        x0 = x;
    }
    printf("\n\n%d次迭代后未达到精度要求.\n",N);
    system("pause");
}
```
