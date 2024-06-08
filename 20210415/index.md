# C++栈结构实现括号匹配


# 题目 括号匹配检验(数据结构严蔚敏C语言版的C++实现)

输入：一串括号              

输出：检验结果（缺左括号、有多余左括号、左右不匹配、匹配成功）              

处理方法：

检验括号是否匹配的方法可用“期待的急迫程度”这个概念来描述：            

后出现的“左括弧”，它等待与其匹配的“右括弧”出现的“急迫”心情要比先出现的左括弧高。对“左括弧”来说，后出现的比先出现的“优先”等待检验。

对“右括弧”来说，每个出现的右括弧要去找在它之前“最后”出现的那个左括弧去匹配。显然，必须将先后出现的左括弧依次保存，为了反映这个优先程度，保存左括弧的结构用栈最合适。    

对出现的右括弧来说，只要“栈顶元素”相匹配即可。如果在栈顶的那个左括弧正好和它匹配，就可将左括弧从栈顶删除。     

步骤：

```
1）凡出现左括弧，则进栈；
2）凡出现右括弧，首先检查栈是否空
若栈空，则表明该“右括弧”多余，
否则和栈顶元素比较，
若相匹配，则“左括弧出栈”，
否则，表明不匹配。
3）表达式检验结束时，
若栈空，表明表达式中匹配正确，
否则，表明“左括弧”有余
```
## 一、问题分析

先对输入字符串进行遍历提取，将左括号入栈，遇到右括号，出栈一个左括号，判断是否出栈的是左括号，如果不是，缺一个左括号，如果是，判断左括号与右括号是否匹配。最后如果栈中还剩余括号，则证明有多余括号，不匹配。

## 二、代码

对于问题三进行求解，设计了match函数，代码如下

```c++
Status match(char *exp)
{
    //检验表达式中所含括弧是否正确嵌套，若是则返回OK,否则返回ERROR 
    SElemType left;
    SqStack S;
    InitStack(S);   //初始化栈S
    char *p=exp;
    char ch = *p;
    //当字符串exp未扫描
    while (ch  != '#' )
    {
        if(is_left(ch))
        {
            Push(S,ch);//左括号进栈
        }
        if(is_right(ch))
        {
            Pop(S,left);
            if(!is_left(left) || left == *"$")
            {
                return FALSE; //缺左括号
            }
            else if(!peidui(left,ch))
            {
                return FALSE; //左右不匹配
            }
        }
        p++;
        ch = *p;
    }
    if(!StackEmpty(S))
    {
        
        return FALSE; //有多余的左括号
    }
    else
    {
        return TRUE;
    }
}
```

## 三、分析

分析：缺一个左括号和缺一个右括号的情况都可以正常识别，这得益于先对左括号和右括号的分类，最后再进行匹配的检验的程序结构。

## 四、实验过程记录

刚看到题目的时候，我想着是使用switch对输入进行匹配检验，后来发现这需要嵌套比对，比较麻烦。后来看到老师对括号进行左括号右括号分类，找到了不需要嵌套比较的方法。最后有发现缺左括号情况栈已经空了，但是仍然匹配的情况，这里栈空后pop函数没有更新left值，修改这种情况以￥标识空栈，完善了匹配识别函数

# 完整代码
```C++
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
typedef char SElemType;
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
    if(S.top==S.base) 
    {
        e = *"$";
        return ERROR;  //栈空
    }
    e=*--S.top; 
    return OK;
}

//括号匹配

//判断左括号函数
Status is_left(char ch)   
{   
    if(ch == '('  || ch=='['  ||  ch=='{' )
    {
        return TRUE;
    }
    else
    {
        return FALSE;
    }
}

//判断右括号函数
Status is_right(char ch)    
{      
    if(ch == ')' || ch==']' || ch=='}') 
    {
        return TRUE;
    }
    else
    {
        return FALSE;
    }
}

//判断是否配对
Status peidui(char a, char b)
{
    if(a == '(' && b == ')')
    {
        return TRUE;
    }
    else if(a == '[' && b == ']') 
    {
        return TRUE;
    }
    else if(a == '{' && b == '}') 
    {
        return TRUE;
    }
    else
    {
        return FALSE;
    }
}

Status match(char *exp)
{
    //检验表达式中所含括弧是否正确嵌套，若是则返回OK,否则返回ERROR 
    SElemType left;
    SqStack S;
    InitStack(S);   //初始化栈S
    char *p=exp;
    char ch = *p;
    //当字符串exp未扫描
    while (ch  != '#' )
    {
        if(is_left(ch))
        {
            Push(S,ch);//左括号进栈
        }
        if(is_right(ch))
        {
            Pop(S,left);
            if(!is_left(left) || left == *"$")
            {
                return FALSE; //缺左括号
            }
            else if(!peidui(left,ch))
            {
                return FALSE; //左右不匹配
            }
        }
        p++;
        ch = *p;
    }
    if(!StackEmpty(S))
    {
        
        return FALSE; //有多余的左括号
    }
    else
    {
        return TRUE;
    }
}

//实验5
void test()
{
    char str[20];
    cout<<"输入括号(#结束):";
    cin>>str;
    bool result = match(str);
    if(result != true)
    {
        cout<<"不匹配";
    }
    else
    {
        cout<<"匹配";
    }
    cout<<endl;
}

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);

    //实验5
    test();

    system("pause");
}

```
