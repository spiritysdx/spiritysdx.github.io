# C++顺序表合并


# 题目 C++顺序表合并(数据结构严蔚敏C语言版的C++实现)

求两个线性表La和Lb的并集La = La U Lb （用顺序表实现）     

( 要求：“就地运算”，运算结果仍然存放在La中 )    

输入：线性表La、线性表Lb      

输出：变化了的La      

处理方法：扩大线性表La，将存在于线性表Lb 而不存在于线性表La中的数据元素插入到线性表La中去      

## 一、问题分析

需要创建两个线性顺序表La和Lb，先从线性表Lb中依次取得每个数据元素，依值在线性表La中进行遍历比较查访，若La中该元素不存在，则在La的表尾插入。  

## 二、代码

对于问题一进行求解，设计了unionL函数，代码如下
```c++
Status unionL(SqList &La,SqList Lb) 
{
// 求各表的长度
    int La_len = ListLength(La);  
    int Lb_len = ListLength(Lb); 
    int e;  //初始化e 
    for (int i = 1;  i <= Lb_len;  i++) 
    {
        GetElem(Lb,i,e); 
        if (!LocateElem(La, e))
        {
        ListInsert(La, ++La_len, e);    
       }
   }
    return OK;
}
```

## 三、分析 

通过读表元函数将Lb中的元素读取出来，再对该元素对La进行下标查找，如果查找不到，在表的最后面插入该元素。

## 四、实验过程记录  

这个程序我在码的过程中难点是操作函数的编写，特别是一些自增自减符，写着写着就忘记是先运算再使用还是先使用再运算了，这给我后面写实际解决方案时带来了很大的困难，没有报错却又找不到哪里有错误使得结果有误。    

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
//初始空间参数
#define     LIST_INIT_SIZE      100      //线性表的初始存储空间
#define     LISTINCREMENT       10       //线性表的增量空间
#include<iostream>
#include<stdlib.h>
using namespace std;

//设置状态码和数据类型
typedef int Status;
typedef int ElemType;

//定义顺序表
typedef  struct SqList{
    ElemType   *elem;        //头指针指向数组首元素
    int     Length;     //线性表的当前长度
    int     Listsize;   //线性表的当前容量
}; 

//初始化
Status InitList(SqList &L)
{
    //给表分配空间

    L.elem = new ElemType(LIST_INIT_SIZE);

    //判断是否生成
    if(!L.elem)
    {
        return OVERFLOW;
    }
    else
    {
        L.Listsize = LIST_INIT_SIZE;   //表长度为初始值
        L.Length = 0;                  //表默认长度0
        return OK;
    }
};

//销毁
Status DestroyList(SqList &L)
{
    if(L.elem != NULL)//判断表是否已存在
    {
    free(L.elem);
    L.elem = NULL;
    L.Length = 0;
    L.Listsize = 0;
    return OK;
    }
    else
    {
        return ERROR;
    }
}

//判空
bool ListEmpty(SqList L)
{
    if(L.Listsize == 0)//如果顺序表的长度为0，则表为空
    {
        return true;
    }
    else
    {
        return false;
    }
}

//求表长
int ListLength(SqList L)
{
    return L.Length;//返回顺序表表长
}

//查找返回下标
int LocateElem(SqList L,ElemType e)  //bool compare(int p,int e)
{
    int *p,i;
    p = L.elem;
    i = 1;
    while (i <= L.Length && *p++ != e)//后置++高于*
        i++;
    if (i > L.Length)
        i = 0;
    return i;
}

//读表元(已经自动转换下标了)
Status GetElem(SqList L,int i,ElemType &e)
{
    if(i<1 || i>ListLength(L))
        return OVERFLOW;
    int *p;
    p = &(L.elem[i-1]);
    e = *p;
    return OK;
    
}

//打印函数
void Print(SqList L)
{
    for (int i = 0; i < L.Length; i++)
    {
        cout << L.elem[i] << "";
    }
    cout << endl;
}

//求前驱
ElemType PriorElem(SqList L,ElemType cur_e,ElemType &pre_e)
{
    if(L.elem != NULL)
        {
        int i = LocateElem(L,cur_e);
        if(i==0 || i ==1)
        {
            pre_e = NULL;
        }
        else
        {
            pre_e = L.elem[i-2];
        }
        return pre_e;
    }
}

//求后继
ElemType NextElem(SqList L,ElemType cur_e,ElemType &next_e)
{
    if(L.elem != NULL)
        {
        int i = LocateElem(L,cur_e);
        if(i==0 || i ==1)
        {
            next_e = NULL;
        }
        else
        {
            next_e = L.elem[i];
        }
        return next_e;
    }
}

//线性表置空
Status ClearList(SqList &L)
{
    if(L.elem != NULL)
    {
        L.Length = 0;
        return OK;
    }
    else
    {
        return ERROR;
    }
}

//赋值
Status PutElem(SqList &L,int i,ElemType &e)
{
    if(L.elem != NULL && !i<1 && !i>ListLength(L))
    {
        L.elem[i] = e;
        return OK;
    }
    else
    {
        return ERROR;
    }
}

//插入元素
Status ListInsert(SqList &L,int i,ElemType e)
{
    //判断是否非法
    if(i<1||i>L.Length+1)
    {
        return OVERFLOW;
    }
    //判断是否满了，如果满动态扩展
    if (L.Length >= L.Listsize)
    {
        //计算新表大小
        int newsize = L.Listsize + LISTINCREMENT;

        //分配新空间
        int *newbase = (ElemType *)realloc(L.elem,newsize*sizeof(ElemType));//已经自动释放原L，并拷贝数据到新空间

        //判断是否成功分配
        if(!newbase) exit(OVERFLOW);

        //更新新空间指向
        L.elem = newbase;

        //更新容量
        L.Listsize += LISTINCREMENT;
    }

    int *q,*p;
    q = &L.elem[i-1];//令q指向ai，静态             
    for(p = L.elem+L.Length-1; p>=q ;--p)//下标从0开始，第几个元素得减1
    {
        //从后往前检索，逐个元素后移
        *(p+1) = *p;
    }
    *q = e;
    ++L.Length;
    return OK;
}

//删除元素
Status ListDelete(SqList &L,int i,ElemType &e)
{
    if(i<1||i>L.Length) 
    {
    return ERROR;  //i非法
    }
    ElemType *p=L.elem+i-1;//p为被删除元素的位置
    e=*p;//被删除元素的值赋给e
    ElemType *q=L.elem+L.Length-1;//表尾元素的位置
    for(++p;p<=q;++p)
    {
        *(p-1)=*p;  //左移一位
    }
    L.Length--;    //表长减1
    return OK;
}

//合并两表
Status unionL(SqList &La,SqList Lb) 
{
    int La_len = ListLength(La); // 求各表的长度 
    int Lb_len = ListLength(Lb); 
    int e;  //初始化e 
    for (int i = 1;  i <= Lb_len;  i++) 
    {
        GetElem(Lb,i,e); // 取Lb中第i个数据元素赋给e
        if (!LocateElem(La, e))
        {
        ListInsert(La, ++La_len, e);    
        // La中不存在和 e 相同的数据元素，则插入
        }
    }
    return OK;
}

//实验1
void test01()
{
    SqList La;
    InitList(La);
    int a;
    cout << "顺序表La的5个元素：" << endl;
    for (int j = 0; j < 5; j++)
    {
        cin >> a;
        ListInsert(La, j+1, a);
    }

    SqList Lb;
    InitList(Lb);
    int b;
    cout << "顺序表Lb的5个元素：" << endl;
    for (int k = 0; k < 5; k++)
    {
        cin >> b;
        ListInsert(Lb, k+1, b);
    }

    unionL(La,Lb);
    cout << "La与Lb的并集为：" << endl;
    int l = 0;
    while(l<La.Length)
    {
        cout<<La.elem[l]<<endl;
        l++;
    }
    cout<<endl;
    DestroyList(La);
    DestroyList(Lb);
}

int main()
{
    //显示中文
    SetConsoleOutputCP(65001);

    //实验1
    test01();

    system("pause");
    return 0;
}
```

