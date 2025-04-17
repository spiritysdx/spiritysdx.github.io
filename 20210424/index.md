# C++数值逼近-复化梯形公式和复化 Simpson 公式计算定积分的近似值

# 数值逼近

将区间```[0,1]```四等分，分别用复化梯形公式和复化 Simpson 公式计算定积分
```
f(x)  4.0 / (1 + (x) * (x))
```
的近似值。

```c++
#include<iostream>
using namespace std;
#include<stdio.h>
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8 
#define  f(x)  4.0 / (1 + (x) * (x))
void test()
{
    float h = (1.0 - 0) / 4, temp, xk, yk, xkh, ykh, xk1, yk1;
    int i;
    temp = f(0); xk = 0;
    for (i = 1; i < 4; i++)
    {
        xk = xk + h;
        temp = temp + 2 * f(xk);
    }
    temp = temp + f(1);
    temp = temp * h / 2;
    cout << endl;
    cout << "复化梯形公式计算的结果：" << temp << endl;

    temp = 0;
    h = (1.0 - 0) / 2;
    xk = 0;
    yk = f(0);
    for (i = 0; i < 2; i++)
    {
        xkh = xk + h / 2;
        ykh = f(xkh);
        xk1 = xk + h;
        yk1 = f(xk1);
        temp = temp + h * (yk + 4 * ykh + yk1) / 6;
        xk = xk1;
        yk = yk1;
    }
    cout << endl;
    cout << "复化 Simpson 公式计算的结果：" << temp << endl;
    cout << endl;
}
int main()
{
    //显示中文
    SetConsoleOutputCP(65001);
    test();
    system("pause");
    return 0;
}
```
