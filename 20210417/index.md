# C++栈结构实现行编辑


# 题目 行编辑(数据结构严蔚敏C语言版的C++实现)

输入：一行有误的数据        

输出：一行正确的数据        

处理方法：              
允许用户输入出差错，并在发现有误时及时更正。例如：可用一个退格符“#”表示前一个字符无效；可用一个退行符“@”表示当前行中的字符均无效。   

步骤：
```
1）初始化栈S
2）读入字符ch
3）ch!=EOF
      3.1) ch!=EOF &&  ch!=‘\n’
              3.1.1)ch为‘#’：Pop(S, c),转3.1.4)
              3.1.2)ch为‘@’：ClearStack (S),转3.1.4)
              3.1.3)ch为其它： Push (S, ch),转3.1.4)
              3.1.4)再读入字符ch,继续3.1)
      3.2) 处理完一行,清空栈
      3.3) 如ch!=EOF,读入字符ch,继续3)
```
## 一、问题分析

需要使用由char类型元素组成的栈，写一个LinkEdit函数对输入进行检索判断，遇到字符类型时判断一下需要进行的对应操作。

## 二、代码

对于问题三进行求解，设计了LinkEdit函数，代码如下

```c++
//识别
void LinkEdit(SqStack& S)
{
    char ch;
    SElemType e;
    ch = getchar();
    while (ch != EOF && ch != '\n')
    {
        switch (ch)
        {
        case '@':
            ClearStack(S);
            break;
        case '#':
            Pop(S, e);
            break;
        default:
            Push(S, ch);
        }
        ch = getchar();
    }
}
```

## 三、分析

这里使用一个新栈对输入后的结果进行输出，避免栈的“先进后出”的结构特点导致直接输出逆序。

## 四、实验过程记录

这个程序我写了一个打印函数，上一个实验直接进行输出了，这个进行了遍历再输出。这个程序我使用了switch结构进行多次判断，这里的判断比较简单，判断后再进行入栈操作。

# 完整代码

```c++
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
#define OK 1
#define ERROR -1
#define OVERFLOW -2
#define TRUE 1
#define FALSE 0
#define STACK_INIT_SIZE  100
#define STACKINCREMENT 10
#define EOF   -1   //C++中需要声明
#include <cstdio>
#include <cstdlib>
#include <iostream>
using namespace std;

//定义数据类型
typedef char SElemType;
typedef int status;

//定义栈
typedef struct 
{
    SElemType* base;
    SElemType* top;
    int stacksize;
}SqStack;

//初始化
status InitStack(SqStack& S) 
{
    S.base = new SElemType[STACK_INIT_SIZE];
    if (!S.base) return(OVERFLOW);
    S.top = S.base;
    S.stacksize = STACK_INIT_SIZE;
    return OK;
}

//判栈空
status StackEmpty(SqStack S) 
{
    if (S.top == S.base) return TRUE;
}

//求栈长
status StackLength(SqStack S)
{
    return S.top - S.base;
}

//求栈顶
status GetTop(SqStack S, SElemType& e) 
{
    if (S.top == S.base) return ERROR;
    e = *(S.top - 1);
    return OK;
}

//入栈
status Push(SqStack& S, SElemType e) 
{
    if (S.top - S.base >= S.stacksize) {
        SElemType* newbase = (SElemType*)realloc(S.base, (S.stacksize + STACKINCREMENT) * sizeof(SElemType));
        if (!newbase) return(OVERFLOW); 
        S.base = newbase;
        S.top = S.base + S.stacksize;
        S.stacksize += STACKINCREMENT;
    } 
    *S.top++ = e;
    return OK;
}

//出栈
status Pop(SqStack& S, SElemType& e) 
{
    if (S.top == S.base) return ERROR;
    e = *--S.top;
    return OK;
}

//清空栈
status ClearStack(SqStack& S)
{
    S.top = S.base;
    S.stacksize = 0;
    return OK;
}

//销毁栈
status DestroyStack(SqStack& S)
{
    S.top = S.base;
    free(S.base);
    S.base = NULL;
    S.top = NULL;
    return OK;
}

//遍历输出
status PrintStack(SqStack S)
{
    SElemType* p = new SElemType[sizeof(SElemType)];
    p = S.top;
    if (p == S.base)
        cout << "栈空" << endl;
    else
    {
        cout << "有效字符为：" << endl;
        p--;
        while (p != S.base - 1)
        {
            cout << *p;
            p--;
        }
    }
    cout<<endl;
    return OK;
}

//识别判断
void LinkEdit(SqStack& S)
{
    char ch;
    SElemType e;
    ch = getchar();//读取字符，读错误返回EOF
    while (ch != EOF && ch != '\n')//EOF为全文结束符
    {
        switch (ch)
        {
        case '@':// 重置S为空栈
            ClearStack(S);
            break;
        case '#'://栈不空，退栈
            Pop(S, e);
            break;
        default:
            Push(S, ch);
        }
        ch = getchar();//读取下一个字符
    }
}

//实验6
void test()
{
    SqStack S, Q;
    SElemType e;//中间变量
    InitStack(S);
    InitStack(Q);
    while (true)
    {
        cout<<"输入："<<endl;
        LinkEdit(S);//识别
        while (StackLength(S))//如果识别判断后不空
        {
            //正序输出转换
            Pop(S, e);
            Push(Q, e);
        }
        PrintStack(Q);
        ClearStack(Q);
    }
    DestroyStack(S);
    DestroyStack(Q);
}

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);
    
    //实验6
test();

system("pause");
    return 0;

}
```
