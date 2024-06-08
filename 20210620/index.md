# 几种数值算法的简单分析

## 前言

感谢组员的共同协作。

## 四阶龙格-库塔公式计算微分方程数值解

```C++
#include<iostream>
#include<iomanip>
using namespace std;

double f(double t, double x, double y)
{
    double dx;  
    dx = x + 2 * y;  
    return(dx);
}  
double g(double t, double x, double y)
{ 
    double dy;  
    dy = 3 * x + 2 * y; 
    return(dy);
}
void LG(double(*f)(double t, double x, double y), 
        double(*g)(double t, double x, double y), 
        double cz[3], double rs[3], double h)
{
    double f1, f2, f3, f4, g1, g2, g3, g4, t0, x0, y0, x1, y1;
    t0 = cz[0]; 
    x0 = cz[1]; 
    y0 = cz[2];
    f1 = f(t0, x0, y0);
    g1 = g(t0, x0, y0);
    f2 = f(t0 + h / 2, x0 + h * f1 / 2, y0 + h * g1 / 2);
    g2 = g(t0 + h / 2, x0 + h * f1 / 2, y0 + h * g1 / 2);
    f3 = f(t0 + h / 2, x0 + h * f2 / 2, y0 + h * g2 / 2);
    g3 = g(t0 + h / 2, x0 + h * f2 / 2, y0 + h * g2 / 2);
    f4 = f(t0 + h, x0 + h * f3, y0 + h * g3);
    g4 = f(t0 + h, x0 + h * f3, y0 + h * g3);
    x1 = x0 + h * (f1 + 2 * f2 + 2 * g3 + g4) / 6;
    y1 = y0 + h * (g1 + 2 * g2 + 2 * g3 + g4) / 6;
    rs[0] = t0 + h; rs[1] = x1; rs[2] = y1;
}

int main()
{
    double cz[3], rs[3]; 
    double a, b, S; 
    double t, step; 
    int i; 
    cout << "输入微分方程的初值:";  
    cin >> cz[0] >> cz[1] >> cz[2];
    cout << "输入所求微分方程组的微分区间[a,b]:";  cin >> a >> b;  
    cout << "输入所求微分方程组所分解子区间的个数:";  
    cin >> step; 
    S = (b - a) / step; 
    cout << cz[0] << setw(10) << cz[1] << setw(10) << cz[2] << endl; 
    for (i = 0; i < step; i++) 
    { 
        LG(f, g, cz, rs, S);  
        cout << rs[0] << setw(10) << rs[1] << setw(10) << rs[2] << endl; 
        cz[0] = rs[0]; 
        cz[1] = rs[1]; 
        cz[2] = rs[2]; 
    }  

    system("pause");
    return 0;
} 
```

## 龙贝格公式计算积分值

```C++
#include<iostream>
#include<math.h>
#define N 20             //区间等分份数
#define MAX 10 
#define a 0
#define b 1
#define f(x) (sin(x))    //函数
#define epsilon 0.0001    //有效数字
using namespace std;

double Romberg(double aa,double bb,long int n)
{
    int i;
    double sum,h=(bb-aa)/n;
    sum = 0;
    for(i=1;i<n;i++)
    {
        sum=sum+f(aa+i*h);
    }
    sum = sum + (f(aa)+f(bb))/2;
    return (h*sum);
}

int main()
{
    int i;
    long int n=N, m=0;
    double T[2][MAX+1];
    T[1][0]=Romberg (a, b,n) ;
    n=n*2;
    for (m=1;m<MAX;m++)
    {
        for(i=0;i<=m;i++)
        {
            T [0][i]=T[1][i];
        }
        T[1][0]=Romberg(a, b, n) ;
        n=n*2;
        for(i=1;i<=m;i++)
        {
            T[1][i]=T[1][i-1]+(T[1][i-1]-T[0] [i-1])/(pow (2, 2*m)-1);
        }
        if ((T[0][m-1]-T[1] [m]) <epsilon)
        {
            cout<<"T="<<T[1] [m]<<endl;
        }
        system("pause");
        return 0;
    }
}
```


## 利用二分法寻找函数

```C++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<math.h>
#define eps 0.001
using namespace std;
double fun(double x)
{
	return x * sin(x) - 1;
}

double dichotomy(double a, double b)
{
	double c = 0.0;
	if ((fun(a) < 0) && (fun(b) > 0))
	{
		while (true)
		{
			c = (a + b) / 2;
			if (fun(c) < 0)
			{
				a = c;
				if (fabs((a - b)) < eps)
				{
					return (a + b) / 2;
				}
			}
			else if (fun(c) == 0)
			{
				return c;
			}
			else
			{
				b = c;
				if (fabs((a - b)) < eps)
				{
					return (a + b) / 2;
				}
			}
		}
	}
	else
	{
		cout << "你输入的a和b不正确" << endl;
		return -1;
	}
}

int main()
{

	double a = 0;
	double b = 2;
	double result = dichotomy(a, b);
	cout << "求解的结果是：" << result << endl;
system ( "pause ");
	return 0;
}
```

## 定期年金方程问题求解

```C++
#include<iostream>
#include <math.h>
using namespace std;
int main()
{
    int k = 1;
    long double p,p1,f,f1;
    cout<<"输入初值p0=";
    cin>>p1;
    cout<<endl;
    while(true)
    {
        f = 500.0 - (pow(1.0+p1,20)-1.0)/p1;
        cout<<"p="<<f<<endl;
        f1 = -(20.0*pow(1.0+p1,19)-pow(1.0+p1,20))/(p1*p1);
        cout<<"p="<<f1<<endl;
        p=p1-f/f1;
        if(fabs(p-p1)<1e-2)
        {
            break;
        }
        p1=p;
        k++;
    }
    cout<<"p="<<p<<endl;
    cout<<"k="<<k<<endl;
    system("pause");
    return 0;
}
```


**完整文档详见：[博客相关资源-数值逼近课设文件](https://www.spiritlhl.top/ziyuan/)**
