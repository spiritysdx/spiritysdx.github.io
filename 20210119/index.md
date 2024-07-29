# C++循环结构补充

## 输入信息控制循环

输入信息控制循环通常控制的是无限循环(循环次数不确定的循环)。

通常使用while(或do while)语句构建无限循环。

统计输入的字符个数  
```c++
#include<iostream>
using namespace std;
int main()
{
    char ch;
    int count=0;
    cout<<"输入字符串，以#结束：";
    while(true)
    {
        cin>>ch;
        if(ch=='#') break;
        cout++;
    }
    cout<<"共输入"<<count<<"个字母。"<<endl<<endl;
    return 0;
}
```

## 循环嵌套

输出乘法九九表：    
二维表，先从上到下输出行，再从左到右输出列。    
需要构建两个循环，外循环控制行，内循环控制列。  
```c++
#include<iostream>
using namespace std;
int main()
{
    int i,j;

    for(i=1,i<=9;i++)
        cout<<i<<"\t"

    cout<<"\n-------------------------------"
        <<"----------------------------------------\n";
    for(i=1;i<=9;i++)
    {
        cout<<i<<"\t";
        for(j=1;j<=9;j++)
            cout<<i*j<<"\t";
        cout<<endl;
    }
    return 0;
}
```

## 其他控制语句

1.break语句

使用于循环中用于跳出或结束循环。

2.continue语句

使用于循环体中，作用是结束本次循环，然后判断循环条件，决定是否进行下一次循环。

实例：
```c++
//运行这段程序可以多次输入学生百分制成绩，并分别对其进行成绩分级，直到输入-1结束程序。
#include<iostream>
using namespace std;
int main()
{
    int score;
    char result;
    while(true)
    {
        cout<<"请输入学生百分制成绩(0~100,输入-1结束):";
        cin>>score;
        if(score==-1)
            break;
        if(score<0||score>100)
        {
            cout<<"输入学生百分制成绩有错，请重新输入！"<<endl<<endl;
            continue;
        }
        switch(score/10)
        {
        case 10:
        case 9:
            result = 'A';
            break;
        case 8:
            result = 'B';
        case 7:
            result = 'C';
        case 6:
            result = 'D';
        default:
            result = 'E';       
        }
        cout<<"百分制成绩"<<score<<"对应的成绩等级为："
        <<result<<endl<<endl;
    }
    return 0;
}
```

## goto语句

goto语句的作用是使程序的执行流程跳转到语句标号所指定的语句。语法格式如下

```
goto<语句标号>
```

上面实例中的`continue`可以用`goto`语句替代。

```
……
    while(true)
    {
  a1: cout<<"请输入学生百分制成绩(0~100,输入-1结束):";
     cin>>score;
     if(score==-1)
            break;
    if(score<0||score>100)
    {
        cout<<"输入学生百分制成绩有错，请重新输入！"<<endl<<endl;
        goto a1;
    }
    ……
    }
……
```

实例：
```c++
//一元二次方程求解
#include<iostream>
#include<math.h>
using namespace std;
int main()
{
    double a,b,c,d,x1,x2;
    cout<<"一元二次方程：ax*x+bx+c=0\n请输入系数a,b,c:";
n1:cin>>a>>b>>c;
    if(a==0)
    {
        cout<<"请重新输入系数a,b,c:";
        goto n1;
    }
    else
    {
        d=b*b-4*a*c;
        if(d>=0)
        {
            x1=(-b+sqt(d))/(2*a);
            x2=(-b-sqt(d))/(2*a);
            cout<<"x1="<<x1<<endl;
            cout<<"x2="<<x2<<endl;
        }
        else
            cout<<"此方程无解：\n";
    }
    return 0;
}
```
注意：`goto`语句的使用会破坏程序的结构，应该少用或不用。


