# C++二叉树中叶子结点的个数与深度的统计

# 题目 统计二叉树中叶子结点的个数，计算二叉树的深度(数据结构严蔚敏C语言版的C++实现)

输入：字符串序列

输出：叶子结点的个数，二叉树的深度

处理方法：
```
1)先序遍历二叉树。在遍历过程中查找叶子结点，并计数。由此，需在遍历算法中增添一个“计数”的参数，并将算法中“访问结点”的操作改为：若是叶子，则计数器增1。     
2)后序遍历二叉树。从二叉树深度的定义可知，二叉树的深度应为其左、右子树深度的最大值加1。由此，先分别求得左、右子树的深度，算法中“访问结点”的操作为：求得左、右子树深度的最大值，然后加 1 。
```
# 一、问题分析
统计叶子结点的个数需要先序遍历二叉树，增加计数器和判断语句统计叶子节点数。          
统计二叉树的深度需要后序遍历二叉树，求得左右两子树深度，判断最大值并加一输出。          
# 二、代码
问题进行求解，分别设计了CountLeaf函数和 BiTreeDepth函数，代码如下
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
//定义节点数
#define        MAX_TREE_SIZE    100
#include<iostream>
using namespace std;

typedef int Status;
typedef char TElemType;

//定义二叉树结构体
typedef struct BiTNode
{
    TElemType data;
    struct BiTNode *lchild,*rchild;
}BiTNode,*BiTree;

//初始化二叉树
Status InitBiTree(BiTree T)
{   
    //含有头结点，头结点的左孩子指向树的根结点, 
    //T为指向头结点的指针
    if (!(T=new BiTNode)) 
    {
        return ERROR;
    }
    T->lchild=NULL; 
    T->rchild=NULL;
    return OK;   
}

//销毁
Status DestroyBiTree(BiTree &T)
{ 
    if(!T) 
    {
    return FALSE;
    }
    else
    {    
       if (T->lchild!=NULL) 
        {
            DestroyBiTree(T->lchild);
        }
        if (T->rchild!=NULL )
        {
            DestroyBiTree(T->rchild);
        }
        delete T;  
        return OK;  
    }
}

//找根节点
BiTree Root(BiTree T) 
{   
    if (T->lchild==NULL)
    {
        return NULL;
    }
    return T->lchild;
}

//清空
Status ClearBiTree (BiTree &T)
{   
    BiTNode *p;    
    p=T->lchild;
    if (p) 
    {   
        DestroyBiTree(p);
        T->lchild=NULL;      
        T->rchild=NULL;  
    }
    return OK;
}

//查深度
int BiTreeDepth(BiTree T)
{
    if(!T)
    {
        return 0;
    }
    else
    {
        int deepth = max(BiTreeDepth(T->lchild),BiTreeDepth(T->rchild))+1;
        return deepth;//空树会返回深度1
    }
}

//打印函数
void visit(TElemType e)
{
    cout<<e;
}

//先序遍历
void PreOrderTraverse(BiTree T, void(*visit)(TElemType))            
{   
    if (T)
    {
        visit(T->data);
        PreOrderTraverse(T->lchild, visit);
        PreOrderTraverse(T->rchild, visit);
    } 
}

//中序遍历

void InOrderTraverse (BiTree T, void(*visit)(TElemType))
{  
    if (T)
    {
       InOrderTraverse(T->lchild, visit);
       visit(T->data);
       InOrderTraverse(T->rchild, visit);
    } 
}

//后序遍历
void PostOrderTraverse (BiTree T, void(*visit)(TElemType))
{  
    if (T)
    {
       PostOrderTraverse(T->lchild, visit);
       PostOrderTraverse(T->rchild, visit);
       visit(T->data);
    } 
}
//按先序创建二叉树
Status CreateBiTree(BiTree &T)
{
    char ch;
    char kg;
    scanf("%c",&ch);        //  输入数据
    kg=getchar();           //  空格
    
    if(ch == '#')
    {   
        //输入#代表此节点下子树不存数据，不继续递归创建
        T = NULL;

    }
    else
    {
        if(!(T = (BiTNode *)malloc(sizeof(BiTNode))))  exit(OVERFLOW);
        T->data = ch;                       //把当前输入的数据存入当前节点指针的data中
        cout<<"请输入"<<ch<<"的左子树: ";
        CreateBiTree(T->lchild);            //递归创建左子树
        cout<<"请输入"<<ch<<"的右子树: ";          
        CreateBiTree(T->rchild);            //到上一级节点的右边递归创建左右子树
    }
    return OK;
}

//求叶子节点个数
void CountLeaf (BiTree T, int &count)
{
    if (T) 
    {
        if ((!T->lchild)&&(!T->rchild))
        count++;  //对叶子结点计数
        CountLeaf( T->lchild, count);  
        CountLeaf( T->rchild, count); 
    }
}

//查深度
//返回二叉树的深度, T为树根的指针
int BiTreeDepth(BiTree T)
{
    int depthval;
    int depthLeft;
    int depthRight;
    if(!T)
    {
        depthval=0;
    }
    else
    {
        depthLeft = BiTreeDepth(T->lchild);
        depthRight= BiTreeDepth(T->rchild);
        depthval= 1+(depthLeft>depthRight?depthLeft:depthRight);
    }
    return depthval;
}

//实验2
void test()
{
    cout<<"请以先序序列输入该树的树根节点(以#替代空缺)：";
    BiTree T;
    CreateBiTree(T);

    cout<<"树的深度是"<<BiTreeDepth(T)<<endl;

    int l = 0;
    CountLeaf(T,l);
    cout<<"树的叶子节点个数为"<<l<<endl;

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

