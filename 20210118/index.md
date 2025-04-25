# C++程序控制结构

## 选择控制语句 if else

这里不像Python，每个分支语句后不需要加`:`，直接加`  `(Tab)后写分支语句，条件判断需要小加括号括起来，Python中是空格加条件，这点也不同。

```c++
if(x>=0)
    cout<<"y="<<1<<endl;
else
    cout<<"y="<<-1<<endl;
```

也可以省去`else`后的语句

```c++
if(x>=0)  cout<<"y="<<1<<endl;
```

又到了经典的找最大值程序2333

```c++
#include<iostream>
using namespace std;
int main()
{
    int a,b,max;
    cout<<"Input a,b:";
    cin>>a>>b;
    if(a>b)
        max=a;
    else
        max=b;
    cout<<"The max is:"<<max<<endl;
    return 0;
}
```

## 用条件运算符?:代替if else语句

```c++
max=a>b?a:b;
```
代替
```c++
if(a>b)
    max=a;
else
    max=b;
cout<<"The max is:"<<max<<endl;
```

代替后的程序如下：

```c++
#include<iostream>
using namespace std;
int main()
{
    int a,b,max;
    cout<<"Input a,b:";
    cin>>a>>b;
/*
    if(a>b)   max=a;
    else      max=b;
    if(c>max) max=c;
    cout<<"The max is:"<<max<<endl;
*/
//用？：运算符实现
    max=(a>b?a:b)>c?(a>b?a:b):c;
    cout<<"The max is:"<<max<<endl;
    return 0;
}
```

这里的if else语句缩减了格式，这是常用书写规范下需要合理选择的，花括号{}不能超过一屏等就是类似的常用书写规范。

### if else语句的嵌套

有两种形式`嵌套if语句`：

```c++
if()
    if()
    else
else
    if()
    else
```

或

```c++
if()
else if()
……
else if()
else
```

无论是何种形式的if else嵌套，需要注意的是if和else的对应关系，有时候为了区分该层if的语句范围，用花括号{}括起来表示。

```
if()
{   if()  语句1}
else   语句2
```

实例(考试成绩分级)

```c++
#inclue<iostream>
using namespace std;
int main()
{
    int score;
    char result;

    cout<<"请输入学生百分制成绩(0~100):";
    cin>>score;

    if(score>=90)
        result='A';
    else
        if(score>=80)
            result='B';
        else
            if(score>=70)
                result='C';
            else
                if(score>=60)
                    result='D';
                else
                    result='E';
    
    cout<<"百分制成绩"<<score<<"对应的成绩等级"<<result<<endl;
    return 0;
}
```

这里成绩通过自上而下的方式筛选，实际也可以自下而上筛选。

## 多路选择控制语句switch

```
switch(测试表达式)
{
    case   常量表达式1:   语句1
    case   常量表达式2:   语句2
    ……
    case   常量表达式n:   语句n
    default:             语句n+1  
}
```

执行顺序：      
1.执行测试表达式得到其值;    
2.在case语句中找到值相等的常量表达式;       
3.如果没有找到相等的常量表达式时，则从"default:"开始执行。          

注意：     
1.switch后的括号`()`内只能是`整型、字符型、枚举型`;         
2.各常量表达式的值不能相等，且次序不影响执行结果;       
3.每个case语句只是一个入口标号，通常只需要执行一个case后的语句，所以每个case选择的最后应该加`break`语句，用来结束整个switch结构;    
4.当若干选择需要执行相同操作时，可以使多个case选择共用一组语句。    

用`switch`重做考试成绩分级：
```c++
#include<iostream>
using namespace std;
int main()
{
    int score;
    char result;
    cout<<"请输入学生百分制的成绩(0~100):";
    cin>>score;

    switch(score/10)
    {
        case 10:
//            result='A';
        case 9:
            result='A';
            break;
        case 8:
            result='B';
            break;
        case 7:
            result='C';
            break;
        case 6:
            result='D';
            break;
        default:
            result='E';
    }
    cout<<"百分制成绩"<<score<<"对应的成绩等级为："<<result<<endl;
    return 0;
}
```

## 循环控制结构

###1.while语句

```
while(测试表达式) 循环体
```

这里判断测试表达式为`true`时，执行循环体，一般这里的循环体会用花括号{}括起来，判断测试表达式为`false`时，循环结束。

求自然数1~100之和。

```c++
#include<iostream>
using namespace std;
int main()
{
    int i=1,sum=0;
    while(i<=100)
    {
        sum+=i;
        i++;
    }
    cout<<"sum="<<sum<<endl;
    return 0;
}
```

程序运行情况：
```
sum=5050
```

## do while语句

形式
```
do  循环体
while(测试表达式);
```

实例：求1~100之和

```c++
#include<iostream>
using namespace std;
int main()
{
    int i=1,sum=0;
    do{
        sum += i;
        i++;
    }while(i<=100);
    cout<<"sum="<<sum<<endl;
    return 0;
}
```

这里do while语句与while语句不同之处在于是测试表达式的判断先后。

有限次循环三要素:     
1.循环控制变量初始化；  
2.循环结束条件；    
3.循环变量更新。    

## for语句

形式
```
for(初始化表达式;测试表达式;更新表达式)
    循环体
```

实例：
```c++
#include<iostream>
using namespace std;
int main()
{
    int i=1,sum=0;
    for(i<1,i<=100,i++)
    {
        sum +=i;
    }
    cout<<"sum="<<sum<<endl;
    return 0;
}
```

程序段：

```
int i=1,sum=0;
for(i<1,i<=100;i++)
{
    sum +=i
}
```
可以缩写成
```
for(int i=1,sum=0;i<=100;i++)
    sum+=i
```

如果省略初始化表达式和更新表达式，只有测试表达式，则完全等同于while语句。

```
for(;i<=60;)
    sum+= i++;
```
等同于
```
while(i<=60)
    sum+= i++;
```


