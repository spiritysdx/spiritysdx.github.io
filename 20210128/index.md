# C++数组

## 前言

这部分类同于Python，比如从0开始，都用`[]`来表示第几个元素。

## 一维数组(score)

形式如下

```
<类型标识符><数组名>[数组长度]
```

实例

```C++
int score[10];

int const N = 10;
int n,m;
int a[N];           //合法，N为常量
double[n],c[m*2];   //非法，n和m是变量
```

如果要访问一维数组的元素，形式如下：

```
<数组名>[下标]
```

这里类似Python，不过下标不能从后往前检索使用负数，这里不同于Python。

实例：

```c++
//声明一个数组，每个元素按顺序赋予从50到70以1递增的数。
int nDate[20];
for(int i=0;i<20;i++)
    nDate[i]=i+50
```

```c++
//生成长度10的连续偶数序列，从2开始，保存在数组中，并输出此数组每个元素的值
#include<iostream>
using namespace std;
int main()
{
    int nEven(10);  //定义用于存放的数组
    int i;
    for(i=0;i<10;i++)
        nEven[i]=i*2+2;     //将生成偶数值赋给数组元素
    for(i=0;i<10;i++)
    {
        cout<<nEven[i]<<"";     //输出数组元素
    }
    cout<<endl;
    return 0;
}
```

## 一维数组的初始化(赋值元素)

形式：

```
<类型标识符><数组名>[数组长度]={元素值按顺序排列用,分开}
或
<类型标识符><数组名>[]={第0个元素值,第1个元素值,……,第n个元素值}
```

注意：

第一种形式不给满元素值，剩下元素默认赋值0，赋值多于数组长度会报错；第二种形式不给定数组长度，赋值多少个就是有几个元素。

赋值时元素值不能为空。

## 多维数组

多维数组不同在于下标的数目，这里以二维数组为主要方向学习。

二维数组命名形式：

```
<类型标识符><数组名>[第1维长度][第2维长度]
```

这里的存储方式是先存储第1维下标为0的元素，再存第1维下标为1的元素。等同于先存第一行，再存第二行。(行为第1维，列维第2维)

访问二维数组形式：

```
<数组名>[第1维下标][第2维下标]
```

二维数组的初始化形式：

```
<类型标识符><数组名>[第1维长度][第2维长度]={
    {第0个第2维数组},
    {第1个第2维数组},
    ……,
    {第n个第2维数组}
}
另一种形式：
<类型标识符><数组名>[第1维长度][第2维长度]={第0个元素值,第1个元素值,……,第n个元素值}
```

这里的注意事项与1维数组相同。

实例：

```c++
#include<iostream>
int main()
{
    int nRow;   //控制行变量
    int nCol;   //控制列变量
    int nMatrix[6][6]={0};  //二维数组声明，且数组元素被赋值为0
    //生成5×5方阵
    for(nRow=0;nRow<5;nRow++)
    {
        for(nCol=0;nCol<5;nCol++)
        {
            if(nRow%2==0)
                nMatrix[nRow][nCol]=nRow*5+nCol+1;
            else
                nMatrix[nRow][4-nCol]=nRow*5+nCol+1;
        }   
    }
    //输出方阵
    cout<<"生成方阵：\n";
    for(nRow=0;nRow<5;nRow++)
    {
        for(nCol=0;nCol<5;nCol++)
        {
            cout<<nMatrix[nRow][nCol];
            if(nMatrix[nRow][nCol]<10)  //控制输出1位数与2位数时的不同间隔
                cout<<"";
            else
                cout<<"";
        }
        cout<<endl;          //没输出一行后换行
    }
    cout<<endl;
    //计算5×5方阵各行及左上右下对角线之和
    for(nRow=0;nRow<5;nRow++)
    {
        for(nCol=0;nCol<5;nCol++)
        {
            nMatrix[nRow][5]+=nMatrix[nRow][nCol];      //计算各行之和
            nMatrix[5][nRow]+=nMatrix[nCol][nRow];      //计算各列之和
        }
        nMatrix[5][5]+=nMatrix[nRow][nRow];
        cout<<"第"<<nRow+1<<"行之和："<<nMatrix[nRow][5]<<"\t\t";
        cout<<"第"<<nRow+1<<"列之和"<<nMatrix[5][nRow]<<endl;
    }
    cout<<"\n左上右下对角线之和："<<nMatrix[5][5]<<endl;
    return 0;
}
```

这里的数组如果声明为静态的，未赋值的自动清零，若数组未声明为静态的，则不赋值时元素不确定。

## 一维数组名作为参数

形式

```
形式1：<类型标识符><函数名>(类型标识符 数组名[],int 长度)
形式2：<类型标识符><函数名>(类型标识符 数组名[长度])
```

实例：

```c++
//计算成绩平均值，定义函数ave()
#include<iostream>
using namespace std;
double ave(int a[],int n);
int main()
{
	const int N=20;
	int nScore[N]={90,88,45,92,76,59,89,93,60,51,91,65,82,74,92,35,66,78,62,91};
	cout<<"学生平均分数为："<<ave(nScore,N)<<endl;
	return 0;
}
double ave(int a[],int n)		
{
	int i,nSum=0;
    for(i=0;i<n;i++)
	nSum+=a[i];
	return ((float)nSum/n);
} 
```

调试可发现主调函数里数组nScore和被调函数的形参数组a的起始地址一样，即数组nScore和形参数组a用同一段内存空间。


## 二维数组的行地址作为参数

既然二维数组中的每一行相当一个一维数组，就可以把二维数组的行地址作为函数参数，实现传递这一行数据的目的。

```c++
#include<iostream>
using namespace std;
double grade(double fArray[],int n);		//评分函数声明
int main()
{
	double fScoreData[5][6]=
	{
		{9.31,9.20,9.00,9.40,9.35,9.20},
		{9.71,9.52,9.50,9.66,9.49,9.57},
		{8.89,8.80,9.10,9.25,8.90,9.00},
		{9.38,9.50,9.40,9.20,9.90,8.90},
		{9.30,8.84,9.40,9.45,9.10,8.89}
	};
	cout.precision(3);				//设置总的有效位数（不是设置小数点后的位数！）
	cout<<"歌手的最后得分为："<<endl;
	for(int nRow=0;nRow<5;nRow++)
	{
		cout<<nRow+1<<" 号选手成绩为：";
		cout<<grade(fScoreData[nRow],6)<<endl;
	}
	return 0;
}
double grade(double fArray[],int n)			//评分函数定义，n作为行向量的长度
{
	double fMark,fMax,fMin;			//定义记录成绩、最高分、最低分的变量
		fMark=fMax=fMin=fArray[0];
	for(int i=1;i<n;i++)
	{
	  if(fArray[i]>fMax) 	fMax=fArray[i];
	  if(fArray[i]<fMin) 	fMin=fArray[i];
	  fMark+=fArray[i]; 	
    }
	return (fMark-fMax-fMin)/(n-2);	//计算出平均分并返回调用函数
}
```

## 数组与字符串

在C++语言中没有字符串变量类型，为了表示字符串，可以使用字符型数组，字符型数组的每一个元素分别保存字符串中的每一个字符。


```
char <数组名>[]="字符串"
char <数组名>[]={'字符串'}
```

有问题的写法：

```c++
char MyStrinq[]={'T','h','i','s',' ','i','s',' ','a',' ','c','o','m','p','u','t','e','r'};
//缺少字符串的结束标志，输出有问题！
cout<<MyStrinq<<endl;
```

正确写法：
```c++
char MyStrinq[]="This is a computer";
char MyStrinq[]={"This is a computer"};
//它们等同于：
char MyStrinq[]={'T','h','i','s',' ','i','s',' ','a',' ','c','o','m','p','u','t','e','r','\0'};
```

说明：      
1.转义字符`\0`(即8进制的0)，表示的是字符串常量中字符串的结束标志。故：```char a[4]={‘t’,‘h’,‘i’,‘s’};cout<<a<<endl; ```缺少字符串的结束标志，输出有问题。       
2.字符型数组也是数组，故赋初值时必须和字符数组的定义在一起。        
```char str1[]; str1[]="abcd"; ```错误！        
3.不能有：```字符型数组名= “字符串”;``` 如:             
```MyStrinq ="abcd";```错误！       
4.字符型数组的长度在声明时就已经确定（字符串中字符个数+1），在使用的过程中不能更改。            
```
const int N = 20;t[N] = "uvwxyz";
```         

转义字符：\ddd:1到3位8进制符号；\xhh:1到2位16进制符号

## 字符串基本操作

在C++中要实现字符串的运算，例如：连接两个字符串，求字符串的长度等，可以：       
1. 编写程序对字符型数组进行操作：通常要借助于循环结构，通过判断当前字符是否为空字符’\0’，来确定是否循环到底。           
2. 直接调用C++的库函数中提供的各种字符串运算的函数。    

```c++
//求字符串的长度
char pString[] = "abcd";
int nSize=0;
while(pString[nSize]!='\0') //从第一个字符开始直到碰到'\0'为止
	nSize++;
```

注意：  
字符型数组的长度:包括'\0'的长度！       
字符串的长度:不包括'\0'的长度！     

```c++
//字符串的复制:将字符数组s中的字符依此赋给字符数组t，直到碰到字符数组s中的'\0'为止
char s[] = "abcd",t[] = "uvwxyz";	int i=0;
while(s[i]!='\0') //从第一个字符开始直到碰到'\0'为止
{		t[i] = s[i];		i++;	}
t[i]='\0'; //注意修改字符串t的结尾！
cout<<t<<endl;
```

注意：          
字符数组t的长度要大于等于s的，否则t[i]访问越界错误！            

注意：      
被连接的字符串t的初始化长度要大于字符串t目前的长度+字符串s的长度+1          

```c++
//将字符串s连接到字符串t的尾部，要先找到字符串t中的‘\0‘,或确定字符串t的长度，然后从t的尾部开始依此将字符串s中字符赋给t；

const int N = 20; //N要足够大！N大于等于s和t的字符数之和+1
char s[] = "abcd",t[N] = "uvwxyz";
int i=0,nSize=0;
while(t[nSize]!='\0') 	
	nSize++;
do{	
	t[nSize++]=s[i++];	
}while(s[i]!='\0');
t[nSize]='\0'; //修改字符串t的结尾！
cout<<t<<endl;
//通常采用while或do-while循环结构访问字符数组
```

## 最后的实例

```c++
//求字符串长度，复制、连接字符串。
#include<iostream>
using namespace std;
int StringLength(char pS[]);
void StringCopy(char pS[],char pD[]);
void StringCat(char pTocat[],char pD[]);
int main()
{
	char str1[]="abcd";
	char str2[5]="";
	char str3[]="efg";
	cout<<"str1："<<str1<<endl;
	cout<<"str2："<<str2<<endl;
	cout<<"str3："<<str3<<endl;
	cout<<"\nstr1的长度："		<<StringLength(str1)<<endl;
	cout<<"\nstr1字符串复制给str2后：";
	StringCopy(str1,str2);
	cout<<str2<<endl;
	cout<<"\nstr3字符串连接到str1后：";
	StringCat(str3,str1);
	cout<<str1<<endl;
	return 0;
}
//注意：一维字符数组作为函数参数时，函数的声明形式与普通一维数组的不同！不需要提供数组长度！

int StringLength(char pS[])
{
	int nSize=0;
	while(pS[nSize]!='\0')	  nSize++;
	return nSize;
}
void StringCopy(char pS[],char pD[])
{
	int nIndex=0;
	while(pS[nIndex]!='\0')	  pD[nIndex]=pS[nIndex++];
	pD[nIndex]='\0';
}
void StringCat(char pTocat[],char pD[])
{
	int nSize=0,nIndex=0;
	while(pD[nSize]!=0) 	  nSize++;
	do{	
     pD[nSize++]=pTocat[nIndex++];
	}while(pTocat[nIndex]!='\0');
	pD[nSize]='\0';
}
```

