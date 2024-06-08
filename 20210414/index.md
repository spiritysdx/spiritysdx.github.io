# C++栈结构实现进制转换


# 题目 数制转换(数据结构严蔚敏C语言版的C++实现)

输入：十进制整数        

输出：八进制整数        

处理方法：基于下列原理：        

```
N = (N div d)×d + N mod d              
```

(其中：div为整除运算，mod为求余运算)       

步骤：（举例说明）（1348)10 = (2504)8      

## 一、问题分析

需要对一个输入变量进行除模运算，将余数压入栈中，将商再赋值给输入变量，再将栈中的元素出栈并输出，这个倒序输出的结果正好是进制转换的结果。

## 二、代码

对于问题三进行求解，设计了translate函数，代码如下   

```c++
void translate(SqStack &S,int a,int &result)
{
    while (a!=0)//取余数N = (N div d)×d + N mod d
    {
        result = a%8;
        a = a/8;
        Push(S,result);
    }
    while (!StackEmpty(S))//从栈中取出结果，先进后出
    {
        Pop(S,result);
        cout<<result;
    }
    cout<<endl;
}
```
## 三、分析

分析：这里的结果是栈“先进后出”结构的一个运用。

## 四、实验过程记录

在完成这个程序时我遇到的苦难在于操作函数的编写，这耗费了我很长时间，后面的进制转换函数比较简单，将数学公式转化为代码即可。

# 完整代码

```c++
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
//函数结果状态代码
#define         TRUE            1
#define         FALSE           0
#define          OK             1
#define          ERROR          0   
#define        INFEASIBLE       -1
#define         OVERFLOW        -2
#define  STACK_INIT_SIZE  100    //存储空间初始分配量
#define STACKINCREMENT 10    //存储空间分配增量
#include<iostream>
using namespace std;

//设置状态码和数据类型
typedef int Status;
typedef int SElemType;

typedef struct 
{
    SElemType       *base;  
    //数组首地址,在栈构造之前和销毁之后，base的值为NULL
    SElemType       *top;   //栈顶指针
    int             stacksize;//当前已分配的存储空间，以元素为单位
}SqStack;

//初始化
Status InitStack(SqStack &S ) 
{
    // 构造一个空的顺序栈 
    S.base=new SElemType[STACK_INIT_SIZE];
    if (!S.base) 
    {
        exit(OVERFLOW);
    } //存储分配失败
    S.top = S.base; //长度为0
    S.stacksize = STACK_INIT_SIZE;//初始存储容量
    return OK;
}

//销毁
Status DestroyStack(SqStack &S)
{
    free(S.base);
    S.top == S.base == NULL;
    S.stacksize = 0;
    return OK;
}

//判栈空
Status StackEmpty(SqStack S)
{
    //若栈空，返回TRUE；否则返回FALSE。
    if(S.top==S.base) 
    {
        return TRUE; //栈空
    }
    else 
    {
    return FALSE;
    }
}

//求栈长
Status LengthStack(SqStack &S)
{
    return (S.top-S.base);//栈顶指针减去栈底指针
}

//清空栈
Status ClearStack(SqStack &S)
{
    S.top=S.base;//让栈顶指针指向栈底位置
    return OK; 
}

//读栈顶
Status GetTop(SqStack S, SElemType &e)
{
//若栈不空，则用e返回S的栈顶元素，并返OK，
//否则返回ERROR
    if(S.top==S.base) 
    {
        return ERROR; //栈空
    }
    e=*(S.top-1);
    return OK;
}


//进栈

//int
Status Push(SqStack &S, SElemType e)
{
//插入元素e为新的栈顶元素      
if(S.top-S.base >= S.stacksize) 
{//栈满，追加空间
    SElemType *newbase=(SElemType *)realloc(S.base, (S.stacksize+STACKINCREMENT)*sizeof(SElemType));
    if(!newbase) exit(OVERFLOW);
    S.base=newbase;
    S.top=S.base+S.stacksize;
    S.stacksize+=STACKINCREMENT; 
} //if
    *S.top++=e; 
    return OK;
}

//出栈
//int
Status Pop(SqStack &S, SElemType &e)
{
//若栈不空，则删除栈顶元素，用e返回其值，并返回OK；
//否则返回ERROR 
    if(S.top==S.base)  return ERROR;  //栈空
    e=*--S.top; 
    return OK;
}

//进制转换
void translate(SqStack &S,int a,int &result)
{
    while (a!=0)//取余数N = (N div d)×d + N mod d
    {
        result = a%8;
        a = a/8;
        Push(S,result);
    }
    while (!StackEmpty(S))//从栈中取出结果，先进后出
    {
        Pop(S,result);
        cout<<result;
    }
    cout<<endl;
}

//实验4
void test()
{
    int input_num;
    int result;
    cout<<"输入十进制数"<<endl;
    cin>>input_num;
    SqStack S;
    InitStack(S);
    cout<<"十进制数"<<input_num<<"转换八进制数为：";
    translate(S,input_num,result);
}

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);

    //实验4
    test();

    system("pause");
    return 0;

}

```
