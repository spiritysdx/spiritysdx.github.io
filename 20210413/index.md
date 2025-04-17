# C++单链表尾插法


# 题目 尾插法(数据结构严蔚敏C语言版的C++实现)

输入：键盘输入      

输出：带头结点的单链表L         

处理方法：待插结点*s插入到最后一个结点之后。       

步骤：          

```
1)获得最后一个结点的位置,使p指向该结点              
2)p->next = new LNode;          
3)p = p->next;              
4)cin>>p->data;             
5)p->next = NULL;           
```

分析：要想获取最后一个结点的位置，必须从头指针开始顺着next链搜索链表的全部结点，该过程的时间复杂度是O( )。如果每次插入都按此方法获取最后一个结点的位置，则整个创建算法的时间复杂度为O( )。              
## 一、问题分析

创建一个单链表，在尾插函数中添加尾指针，将输入获的int类型数据元素一个个尾插到单链表最后一个。           

## 二、代码

对于问题三进行求解，设计了Tail_Insert函数，代码如下

```c++
void Tail_Insert(LinkList &L)
{
    int e;
    LNode *p, *s;
    L = new LNode;
    L->next = NULL;//定义头结点
    p = L;         //p是L的尾指针
    cout<<"请输入节点(输入0结束并输出结果)："<<endl;
    cin >> e;      //从键盘输入一个结点
    while (e != 0) 
    {
        s = new LNode;
        s->data = e;
        s->next = NULL;
        p->next = s;
        p = s;      //插入链表
        cin >> e;   //继续读
    }
}
```
## 三、分析

通过尾插法插入后，输入的数字是按照倒序在栈S中存储的，如果使用S进行输出，输出结果会是倒叙，需要再创建一个栈Q，将栈S中的元素顺序倒置，再由打印函数输出。

## 四、实验过程记录

我在码这个程序时碰到的困难是对指针节点的使用，试过在链接各节点时顺序弄反了，导致数据丢失了，后面经过一段排查后程序正常运行了。

# 完整代码
```c++
//显示中文
#define _CRT_SECURE_NO_WARNINGS
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
#define INFEASIBLE -1
#define OVERFLOW -2
#include<iostream>
using  namespace std;

typedef int ElemType;
typedef int status;

//创建节点结构体
typedef struct LNode 
{
	ElemType   data;
	struct  LNode *next;
}LNode, *LinkList;

//初始化
status InitList(LinkList&L) 
{
	L = new LNode;
	if (!L) exit(OVERFLOW);
	L->next = NULL;
	return OK;
}

//
status Getelem(LinkList L, int i, ElemType&e)
{
	LNode *p; int j;
	p = L->next; j = 1;
	while (p&&j < 1)
	{
		p = p->next;
		j++;
	}
	if (!p || j > i)return ERROR;
	e = p->data;
	return OK;
}

void Tail_Insert(LinkList &L)
{
	int e;
	LNode *p, *s;
	L = new LNode;
	L->next = NULL;//定义头结点
	p = L;         //p是L的尾指针
	cout<<"请输入节点(输入0结束并输出结果)："<<endl;
	cin >> e;      //从键盘输入一个结点
	while (e != 0) 
	{
		s = new LNode;
		s->data = e;
		s->next = NULL;
		p->next = s;//插入链表
		p = s;		//更新尾指针
		cin >> e;   //继续读
	}
}

//打印
void PrintList(LinkList L)
{
	LinkList tem = L;
	while (tem->next != NULL)
	{
		tem = tem->next;
		cout << tem->data << " ";
	}
}

//实验3
void test()
{
	LinkList L;
	Tail_Insert(L);
	cout << "输出：";
	PrintList(L);
	cout<<endl;
}

int main()
{
	//显示中文
    SetConsoleOutputCP(65001);
	
	//实验3
	test();
	
	system("pause");
	return 0;
}
```

