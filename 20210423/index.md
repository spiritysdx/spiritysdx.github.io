# C++数值逼近-Newton插值法


# 数值逼近

天安门广场升旗的时间是日出的时刻，而降旗的时间是日落时分。根据天安门广场管理委员会的公告，某年 10 月份升降旗的时间如下： 
```
日期     1                  15                22
升旗   6:09                6:23               6:31
降旗   17:58               17:36              17:26
```
请根据上述数据构造 Newton 插值多项式，并计算当年 10 月 8 日北京市的日照时长。

```c++
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8 
#include<iostream>
#include<stdio.h>
#include<math.h>
#define n 3 //节点个数
#define f(x) 4.0/(1+(x)*(x)) 
using namespace std;
 

//Newton插值多项式
void test01()
{
    int i,j,xx,x[3] = {1,15,22};
    float y[3] = {17-6+(58-9)/60.0,17-6+(36-23)/60.0,17-6+(26-31)/60.0};
    float N[3][3],yy,temp;

    for(i=0;i<n;i++)
    {
        N[i][0] = y[i];   //0阶差商
    }

    for(j=1;j<n;j++)
    {
        for(i=j;i<n;i++)
        {
            N[i][j] = (N[i][j-1]-N[i-1][j-1])/(x[i]-x[i-j]);   //构造差商
        }
    }
    xx = 8;
    yy = 0.0;
    temp = 1.0;
    for(i=0;i<n;i++)
    {
        yy = yy + N[i][i]*temp;
        temp = temp*(xx-x[i]);
    }
    printf("\n这年10月%d日北京日照时长为%7.4f小时.\n",xx,yy);
    printf("即%d小时%3.0f分.\n",(int)floor(yy),60*fmod(yy,1.0));
}

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);

    test01();
    system("pause");
    return 0;
}

```
