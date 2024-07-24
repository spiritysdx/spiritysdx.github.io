# C++自定义数据类型


## 结构体

C++里数组是由相同数据类型的一组元素构成的集合，但实际问题中，经常会遇到由相互关联的，但类型不同的数据组成的数据结构。比如：描述一个学生的数据实体应该包括其学号、姓名、性别、年龄、成绩等数据。如果用多个独立的简单数据类型表示的话，就无法体现其整体性，也不便于数据的整体操作。

C++语言提供了结构体类型对这样的数据结构进行描述。

这里类似Python中的字典嵌套其他类型的键值对。

形式

```
struct  <结构体类型名>
{
<类型标识符>  成员名l;
<类型标识符>  成员名2;
…………
<类型标识符>  成员名n;
};
```

例如：

```c++
struct Student			//定义学生结构体类型
{
	unsigned int no;		//学号
	char name[20];   		//姓名
	bool sex;			//性别
	int math;			//高数成绩
	int english; 		//英语成绩
	int computer; 		//计算机成绩
	float aver; 			//平均成绩
};
```

注意：      
1.结构体定义时，使用关键字struct,结构体类型的命名应符合C++标识符命名的规则，之后就可以用定义好的结构体类型的名字，与C++中的基本数据类型一样，也可以用来申明该结构体类型的变量。     
2.花括号内是对该结构体各个成员的类型声明，成员声明的形式与变量定义的形式类似，成员可以是基本数据类型，也可以是另一个诸如结构体这样的复杂数据类型；      
3.C++语言规定，结构体类型成员列表中不能包含自身,即不能递归定义，但可以包括其他结构体类型，或者是同类型的结构体指针。        
4.结尾处的分号不能省略。         
5.若同一个结构体类型要被多个源文件使用，则通常将这些结构体类型的定义集中存放在一个头文件中，使用该结构体的源文件只要用#include命令包含此头文件即可，不必再每个源文件中重复同样的结构体定义。        
6.结构体类型定义只是说明了一个数据结构的模型，并没有定义实例，也不要求分配实际的存储空间。只有在定义结构体变量时，C++才为其分配内存。       

## 声明结构体变量

通常使用形式一，形式二是C语言形式。

形式：

```
形式一：<结构体类型名>  变量名
形式二：struct  <结构体类型名>  变量名
```

声明结构体变量也可在定义结构体类型的同时进行：

```
struct 结构体类型名
{
<类型标识符>  成员名l;
<类型标识符>  成员名2;
…………
<类型标识符>  成员名n;
}变量1,变量2,…… ,变量n;
```

实例：
```c++
struct date
 {   
     int year;
     int month;
     int day;
  }d1,d2; 
date d3,d4;
```

若省略结构体类型名，即成为无名结构体类型,无法再次使用该无名结构体类型再定义新的变量。 

## 结构体长度

结构体的长度是各个成员在内存中所占字节数之和。例如：
Student stu;
则stu将在内存中占用41个字节，其中：
```c++
unsigned int no;		//4个字节
char name[20];   		//20个字节
bool sex;			//1个字节
int math; 			//4个字节
int english; 			//4个字节
int computer; 			//4个字节
float aver; 			//4个字节
```

## 结构体变量的初始化

结构体变量的初始化形式为：
```
<结构体类型>  变量名={表达式1,表达式2,…,表达式n}
```

注意：            
1.表达式各类型要与结构体成员的类型对应一致。         
2.只可在声明结构体变量时对其进行初始化，不可在定义结构体类型时，对结构体变量初始化。         

```c++
//错误写法如下
struct date
{   
     int year;
     int month;
     int day;
 }d1;
d1 ={2000,1,1};
//结构体的变量定义和初始化也不能分开
//错误写法如下
struct date
{   
     int year=2000;
     int month=1;
     int day=1;
 };
```

## 结构体变量引用其成员的形式

访问一个结构体变量的成员的形式是：

```
<结构体变量名>.<成员名>
```

其中，```“.”称为成员运算符```，它的优先级高于所有的算术运算符、条件运算符和逻辑运算符。

例子：

```c++
Student stu={20130101,”XuXin”,false,86,90,98};
stu.ave=(stu.math+stu.english+stu.computer)/3;
```

如果一个结构体中包含另一个结构体作为其成员，则访问该成员中的成员时，要使用如下形式：

```
<结构体变量名>.<结构体成员名>.<成员名>
```

例如在学生结构体Student中有一个成员表示学生的出生日期birthday，其类型是如下Date结构体:
```c++
struct Date
{   
     int year;
     int month;
     int day;
};
struct Student			//定义新的学生结构体类型
{
	unsigned int no;		//学号
	char name[20];   		//姓名
	bool sex;			//性别
	Date birthday; //出生日期
	int math;			//高数成绩
	int english; 		//英语成绩
	int computer; 		//计算机成绩
	float aver; 			//平均成绩
}stu1;
```

结构体变量本身不代表一个特定的值，因此无法直接对结构体变量进行算术运算、比较运算和逻辑运算，但作为一个整体结构体可以作为函数参数，同类型的结构体之间也可以相互赋值，其赋值规则是按成员依次进行赋值，如：
```date d1,d2={2001,1,15}; d1 = d2;```

## 结构体数组

多个同一类型的结构体变量也可以用数组的形式来处理，称为结构体数组。

## 共用体

C++允许同一段内存空间中存储不同类型的数据，用于节省内存空间或数据共享。这就需要使用共用体类型。

共用体在同一段内存空间存储不同类型的数据。共用体类型的定义：

```
union  [共用体类型名]
{
	<类型标识符> 成员名1；
	<类型标识符> 成员名2；
	…………
	<类型标识符> 成员名n；
}[变量1,变量2,……,变量n];
```

例如：
```
union UnionDate
{
	int nIntData;
	char cCharData;
	float fRealData;
};
```

### 共用体类型的定义和结构体类型定义很相似，不同之处在于结构体中的成员所占有的存储空间是各自分开，且整体连续的，而共用体的成员所占用的空间是共享的，即共用体的各个成员拥有同样的起始地址。因此对共用体中一个成员的赋值会影响到其他所有成员，且共用体所占空间的长度由其中所占空间的长度最长的成员决定。

声明共用体变量的形式为：	

```
<共用体类型名>  变量名
```

注意：共用体变量不允许赋初始值。

共用体变量中成员的访问形式为：

```<共用体变量名>.<成员名>```

## 共用体与结构体的区别

```c++
struct Student
{
	int no;
	char name[15];
	int score[3];
	float aver;
} stu;
union Utest
{
	char a;
	int b;
	char c[15];
} uni;
cout<<"结构体变量stu的长度："<<sizeof(stu)<<endl;
cout<<"共用体变量uni的长度："<<sizeof(uni)<<endl;
```

注意：由于cpu的对界问题，sizeof返回的和实际长度不一致。这里采用4对齐。

```c++

struct S
{
    int c1;
    int c2;
};
union U
{ 
    int   a;
    int	   b;
    S     d;
}; 
int main()
{ 
  U g;  
  g.b=10;
  g.b=g.a+20;
  g.d.c1=g.a+g.b;
  cout<<g.a<<", "
  <<g.b<<", "<<g.d.c1<<endl;
	return(0); 	
}
```

说明：		
结构体类型S的长度为4+4=8字节，又由于结构体变量d是共用体类型U中占空间最长的成员，所以共用体变量g的长度为8个字节。其中a、b以及结构体变量d的成员c1占据同一段内存空间。因此：当g的成员b被赋值10后，g的成员a和d的成员c1也均为10，所以经过运算后，g.a、g.b、g.d.c1均为60. 

## 枚举类型

枚举类型是一系列有标识名的整型常量的集合。枚举类型也是唯一允许用符号代表数据的数据类型，定义形式如下：

```	
enum  <枚举类型名>{〈枚举常量表〉}[枚举变量]
```
例如：				
```c++
enum Days{Sun ,Mon,Tue,Wed,Thu,Fri,Sat} today;
```

说明：缺省时，系统为每一个枚举常量都对应一个整数，并从0开始，逐个增1，这些缺省的值也可重新指定，例如：

```c++
enum Days{Sun ,Mon,Tue=4,Wed,Thu=8,Fri,Sat}today;
```

则各枚举常量对应的整数依次为0,1,4,5,8,9,10。

注意：枚举变量的取值只能是枚举常量表中的某个枚举常量，而不能用一个整型数据或其它类型数据直接赋值。例如：

```c++
	today=Mon;		//合法，值为1
	today=3;		//不合法，不能直接赋值,
   today=(enum Days)3;//可以,也可以today=(Days)3;即强制类型转换！
	int i=today; 	// 枚举变量直接赋值给整型变量，合法，值为3
```

注意：不要在定义枚举类型的同时，再对枚举常量、枚举变量或枚举类型名重新定义，例如下列定义均```不合法```：		
```c++
int today; int Sun; 
```

## 实例

五色球中取三种不同颜色球的组合

```c++
#include<iostream>
using namespace std;
int main()
{
enum color{red,yellow,blue,white,black};
int count=0;
int i,j,k,t,temp;
for(i=red;i<=blue;++i)
	for(j=i+1;j<=white;++j)
	{
   		 for(k=j+1;k<=black;k++)
		{
       			++count;
			for(t=0;t<3;++t)
			{
				switch(t)
				{
				case 0:					temp=i;break;
				case 1:					temp=j;break;
				case 2:					temp=k;break;
				default:
					cout<<"Impossible\n";
				}
			switch((enum color)temp)
			{
				case red:
				cout<<"red\t";break;
				case yellow:
				cout<<"yellow\t";break;
				case blue:
				cout<<"blue\t";break;
				case white:
				cout<<"white\t";break;
				case black:
				cout<<"black\t";break;
				default:
				cout<<"Impossible\n";
     		}
  			}//for t
  		cout<<endl;
 		} //for k
	} //for j
cout<<"共有"<<count<<"种组合\n";
return(0); 
}
```

## 类型自定义语句

用户使用typedef语句可以将一个标识符定义为数据类型标识符。typedef语句的形式是：	

```
typedef<已有的类型名><类型别名>
```

注意：typedef语句只可以将一个已有的类型名用一个新的类型名来代替；
例如：
```
typedef float FLOAT;
typedef char CH10[10]; //注意写法，不能用typedef char[10] CH10; 
```
则可以：
```
FLOAT x,y;
CH10 str;			//str是具有10个元素的字符型数组
```

它们等价于： 

```
float x,y; char[10] str;
```		

又例如：

```c++
struct complex
 {
   int real;
   int imag;
 };
typedef complex comp;
comp cc ={1,2};
```

或：

```c++
typedef struct complex
 {
   int real;
   int imag;
 }comp;
comp cc ={1,2};
```

## 类和对象

类是面向对象程序设计的基础和核心，也是实现数据抽象的工具。

类实际上是一种特殊的用户自定义数据类型，但与一般的数据类型不同的是，类中不仅包含有一组数据，还有一组函数。

定义好的类，就可以用来声明变量，类的变量习惯称之为类的对象。

## 类的定义

类定义的一般形式为：

```
class  <类名>
{
	Private:		<私有成员函数和数据成员的声明>
	Public:		<公有成员函数和数据成员的声明>
};
<各个成员函数的定义>
```

一般类中成员函数的原型声明写在类定义体内，而成员函数的定义一般写在类定义之外。通常采用下面的形式定义成员函数：

```
<类型标识符><类名>::<成员函数名>(形参表)
	{		 <函数体> 	}
```

作用域运算符（::）指出该成员函数所属的类

```c++
class  CRectangle
{
private:
	int  length,width;
public:
	CRectangle(int l,int w)	{	length=l;width=w;	}
	int get_area(); 
};
int CRectangle::get_area()
{
	return  length*width;
}

```

说明：		
C++定义类时，可以将类的各个成员划分为不同的访问级别，即可以设置三种访问控制属性：public表示成员是公有的；private表示成员是私有的；protected表示成员是受保护的；			
公有成员可以在程序中任何地方访问；而私有成员和保护成员都只有在类的内部能够访问（提高数据的安全性！）
设置三种访问控制属性的关键字： public,private,protected在类内出现的先后次序无关，并可以多次出现，但一个成员只能有一种访问控制属性。		

## 对象的使用

类与对象的关系，相当于普通数据类型与其变量的关系。类是一种抽象概念，一个类只是定义了一种新的数据类型，而只有申明对象时才真正创建了这种数据类型的物理实体。如：

```c++
 class CRectangle
   { private:
     int length = 0; //错误
	  int width = 0;  //错误
   };
```

对象声明的一般形式为：```<类名>  <对象名>(实参表)```			
如： 

```c++
CRectangle c1(10,20);
```

可以用成员访问运算符“.”来引用类的成员，其一般形式为：

```
<对象名>.<数据成员名>  

或：	

<对象名>.<成员函数名(实参表)>
```
如： 
```c++
c1.get_area(); 
```

注意：一般只有类的公有成员才能使用类成员访问运算符“.”，类的保护或私有成员不允许在类的外部被直接访问，只能通过类的公有成员函数来间接访问它们。

get、set函数

也可以通过设置友元（friend）,在类中为友元函数或友元类设置后门，允许它们访问类的保护或私有成员。

```c++
//通过长方形类的成员函数求长方形的面积。
	class  CRectangle
	{
	private:
		int  length,width;
	public:
				//直接定义在类主体中的成员函数将自动成为内联函数（Inline Function）
		CRectangle(int l,int w)		{	length=l;width=w;	}
		int get_area(); 
	};
	int CRectangle::get_area()
	{
		return  length*width;
	}
	int main()
	{
		CRectangle c1(10,20);
		cout<<"The area of rectangle:"<<c1.get_area()<<endl;
		return 0;
	}
```

## 构造函数与析构函数

1.构造函数

构造函数的作用是在对象创建时使用特定的值构造对象，将对象初始化为一个特定的状态。构造函数的名字与它所属的类名相同，被声明为公有函数，且没有任何类型的返回值，在创建对象时被自动调用。例如：

```c++
	CRectangle(int l,int w)	{	length=l;width=w;	}
```

每个类都必须有构造函数，若类定义时没有定义任何构造函数，编译器也会自动生成一个不带参数的缺省构造函数。当创建一个对象时，系统将自动调用该缺省构造函数来初始化对象，缺省的构造函数将对象的所有数据成员分配内存空间，但不进行初始化(数据成员的值为内存的随机值，所以必须提供公有set函数，设置私有数据的值）。

# 不建议这样使用！

构造函数可以有默认的参数值,如：

```c++
CRectangle(int l=1,int w=1){length=l;width=w;}
CRectangle c1;
```

则
```c++
c1.length=c1. width=1;
```

构造函数可以有多个重载版本,例如：


若已经在类外定义了结构体：
```c++
struct rect{int ll;int ww;};
```
则可以： 
```c++
CRectangle(rect rc) {length=rc.ll;width=rc.ww;}
```

拷贝构造函数是构造函数的特例，它的作用是自动用一个已经存在的同类对象，去初始化另一个新的对象。		
语法形式为：```类名(类名 &对象名);```
拷贝构造函数是构造函数的重载形式，它的形参是本类对象的一个引用，实参为一个已存在的本类对象。			
C++自动为每个类提供一个缺省的拷贝构造函数；		
例如：
```c++
CRectangle c1(10,20),c2(c1);
```

2.析构函数

析构函数的功能是用来释放一个对象的，析构函数本身并不删除对象，而是进行系统放弃对象之前的清理工作，它与构造函数的功能正好相反。

注意：		

析构函数的名字是在类名前加字符“～”。

析构函数没有参数，也没有返回值。		
析构函数不能重载，也就是说，一个类中只可能定义一个析构函数。			
例如：			
```c++
	~CRectangle();
```

## 最后一个实例

```c++
//CRectangle类的完整代码
#include<iostream>
using namespace std;
struct rect
{
	int ll;
	int ww;
};
class  CRectangle
{
private:
	int  length,width;
public:
	CRectangle(int l=1,int w=1){length=l;width=w;}
	CRectangle(rect rc); 
	CRectangle(CRectangle &rc); 
	~CRectangle(){ }
	int get_area(); 
	void set(int l,int w);
	void get(int &l,int &w);
};
CRectangle::CRectangle(rect rc) 
{
length=rc.ll;
width=rc.ww;
}
CRectangle::CRectangle(CRectangle &rc) 
{
length=rc.length;
width=rc.width;
}
int CRectangle::get_area()
{
	return  length*width;
}
void CRectangle::set(int l,int w)
{length = l;
width = w;
}
void CRectangle::get(int &l,int &w)
{l = length;
w = width;
}

int main()
{
	rect rr={1,2};
	CRectangle c2(rr);

	CRectangle c1(c2);
    //c1.length=2; //无法存取类中的私有数据成员
    //c1.width=3;  //无法存取类中的私有数据成员
	c1.set(2,3);
	cout<<"The area of rectangle:"<<c1.get_area()<<endl;

	int ll,ww;
	c1.get(ll,ww);
	cout<<"length="<<ll<<“,width="<<ww<<endl;

	//cout<<c1.length<<endl;
	//无法存取类中的私有数据成员
	return 0;
}

```







