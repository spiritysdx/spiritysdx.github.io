# C++指针


# 指针的概念

```内存地址```         
内存空间的编排是以字节为单位的，每一个字节（也称单元）都有一个编号，这就是地址。内存地址通常是一个大小为4个字节的整数（这与具体的计算机有关），一般用十六进制数表示。

```指针```              
指针变量是一种用来存放地址的特殊变量，简称为指针。例如：            
```c++
int * iPointer;
```
编译时，系统将为指针变量iPointer分配另一个内存空间，用于存放某个变量的起始地址。	        
例如： 
```c++
iPointer = &i;
```	
则：指针变量iPointer里存放的就是变量i的起始地址    

```直接访问和间接访问```                
1.直接访问：按变量存取值。          
根据变量的地址直接访问变量的内容。          
变量名经编译后自动转换为变量的地址，直接访问时对变量的存取都是直接通过变量地址进行的。          
例如，运算式k=i+j。             
2.间接访问：               
将变量a的地址存放在变量b中，先访问变量b得到变量b的值（即变量a的地址）后，再访问变量a。          
对变量内容的访问要经过两次：先访问地址，后访问内容。            
例如，先访问iPointer，即找到存放“i的地址”的单元，从中获得i的起始地址0x20000000，然后到0x20000000起始的4个字节中取出i的值。              

# 指针的变量

指针变量简称指针，指针变量保存的是另一个变量b的地址；                  
实际上，这个地址就是一个有特殊意义的32位整数，它是系统为变量b分配的存储空间的起始地址，简称首地址。             
在C++中，可以通过指针操作直接访问存储器中的单元。           

## 指针变量的声明

指针变量声明的一般形式为：	
```
<类型标识符>   *指针变量名              
```
注意：                  
(1)申明指针变量时的 * ,只是一个标志，不表示取内容运算。             
(2)指针变量名必须符合标识符的命名规则；         
(3)申明指针变量时的数据类型是指针变量所指的变量的数据类型；             
(4)申明指针变量时,”*”前后可以加空格，也可以不加；               
比如：```int *p1;``` 或者```int* p1; ```都可以                   
注意：                
int p1, *p2; //p1是int变量，p2是指向int的指针           
int* p1, p2; //p2是int变量，p1是指向int的指针           
建议*紧跟指针变量名。                   

指针声明时需要特别注意：                   
（1）指针变量具有数据类型             
虽然指针存放的都是内存地址，但一个指针变量只能存放声明中指定类型数据的地址，否则编译时会出错。          
例如：````int *ip; float * fp; ```      
则：ip只能指向int型变量、fp只能指向float型变量。                
（2）void*型指针            
void*型指针是一个类型不确定的指针。根据需要可以将void*型指针强制转换成int或其它类型的指针。             
（3）空指针NULL         
C++语言中定义了一个符号常数NULL用来代表空指针，用来表示“无效”的指针值，其值为0。            

## 指针的基本操作

1.  &变量名         
&为取地址运算符，用来获取变量的首地址。

2.   *指针变量名            
这里的星号“*”为指向运算符（也称之为：取内容运算符、间接访问运算符），用来获取指针变量所指向的变量的值。     

“&”和“*”运算符都是一元运算符，其优先级高于所有二元运算符，采用从右到左的结合性。        

例如：          
```c++
int i=5,j=3, *pInt;
pInt = &i;
j=*pInt;
```

注意其中的差别：                
第1句话中*pInt是申明指针变量，*只是一个标志；           
第2句话取i的地址为已经申明过的指针变量赋值，pInt前不再加*           
第3句话中*pInt表示取出指针pInt所指内存中的值，*表示取内容运算        

体会差别：
```c++
int *pInt = &i;
pInt = &i;
 *pInt = j; 
```

## 指针变量的初始化

指针变量也可以在声明时赋初值，完成指针变量的初始化。            
注意：          
（1）对指针变量所赋的初始值必须是同类型变量的地址；           
如：        
```c++
int i=5,j, *pInt=&i; //可以
double f=0.3; int *pInt=&f; //不可以
```

（2）指针变量使用之前必须做初始化（赋初值），即把某个普通变量的地址赋给它。否则指针变量的值就是不确定的，也称之为指针的指向不定，那么贸然使用，就会导致运行错误！           
例如：
```c++
int  a=0; 
int  *x;
 *x=a; //错误！指向不定时无法为x所指的变量赋值！
 a=*x; //错误！指向不定时无法读取x所指的变量！（即无法获取指针变量所指向的变量的值）
```

```c++
//指针变量的定义和使用
	#include<iostream>
using namespace std;
int main()
{
	int m,n,&k=m,*p1=&m,*p2=&n,*pInt=NULL;
	k=n=6;
	cout<<"m="<<m<<" ， n="<<n<<" ， k="<<k<<endl;
	cout<<"&m="<<&m<<" ， &n="<<&n<<" ， &k="<<&k<<endl;
	cout<<"p1="<<p1<<" ， p2="<<p2<<" ,  pInt="<<pInt<<endl;
	cout<<"*p1="<<*p1<<" ， *p2="<<*p2<<endl;
	*p1+=3;
	p2=p1;
	*p2*=4;
	pInt=p2;
	cout<<"*p1="<<*p1<<" ， *p2="<<*p2<<endl;
	cout<<"p1="<<p1<<" ， p2="<<p2<<endl;
	cout<<"m="<<m<<" ， n="<<n<<endl;
	cout<<"pInt="<<pInt<<endl;
	return 0;
}
```

## 指针的赋值运算

（1）指针变量在赋值时，不允许直接将与指针变量类型不同的指针值赋予指针变量。         
例如：    
```c++      
int *iPointer, iValue;char ch='A';
iPointer= &ch; //不允许
iValue = ch; //允许,将字符变量ch对应的ASCII码（整型数据）赋值给iValue
```

（2）用数据类型不兼容的指针指向该数据类型的变量，可以使用强制类型转换。             
如上例中，可以：
```c++
iPointer=(int *)&ch;//允许
```
注意：iPointer被强制指向了ch的起始地址，但ch是字符变量，因此若取指针iPointer所指的内容，则只有最低一个字节内有值（65），高位的三个字节都是随机值（调试显示：*iPointer=-858993599）。            

（3）将其他的任何一种指针值赋予void*型指针变量都必须通过强制类型转换。	 
如：
```c++
void *vPointer = (void *)iPointer; 
```

（4）通过强制类型转换，也可以将指针变量赋值给一个整型变量，或将一个整型常量赋予一个指针变量。例如：

```c++
   iValue = (int)iPointer;  	
	iPointer=(int *)0x98980000;//但这样的地址通常不能做取内容运算！
```

但下面两种做法是错误的：

```c++
int *iPointer = &100;
int *iPointer = &(100*20+iValue);
```

## 指针的算数运算

对于非`void*`型的指针变量，只能进行加一个整数、减一个整数和两个指针变量相减3种运算。`void*`型指针变量`不能做任何算术运算`。                       
对非void* 型的指针变量做加减整数的运算，实际上就是对地址进行加法或减法运算，但这种加减是按照指针所指变量的类型所占的字节数为单位进行的。                     
例如：	
```c++
int iValue; 	
int* iPointer1=&iValue，*iPointer2;	    iPointer2=iPointer1+1;
```
假设iValue的初始地址是0x20000000，即iPointer1的值为0x20000000，则iPointer2=iPointer1+1后，iPointer2的值为0x20000004（注意不是0x20000001）
指针的自加（++）与自减（--）运算，即：  
```c++
iPointer2++; 	
--iPointer1;
```
分别等价于： 
```c++
iPointer2 = iPointer2+1； 
iPointer1 = iPointer1-1；
```
 
## 指针的关系运算

指针间的关系运算结果就是两个指针所指的地址值的大小的关系运算结果。两个进行关系运算的指针一般要求是同一类型的指针。

```c++
#include<iostream>
using namespace std;
int main(){
int n=3,*p1=&n,*p2=NULL;
cout<<"p1="<<p1<<" ， p2="<<p2<<endl;
p1=p2+4; //以int所占字节数为单位进行加减，注意地址采用16进制表示。
p2++;	
cout<<"p1="<<p1<<" ，p2="<<p2<<endl;
p2=p1-2; 	
cout<<"p1="<<p1<<" ， p2="<<p2<<endl;
return 0;
```

## 指针与数组

在声明一个数组时，C++语言将在内存中开辟两个空间，一个用于保存数组元素，另一个用于保存数组的第1个元素的地址，称为数组的首地址，数组名就是保存了数组首地址的指针。

因此对数组元素的访问除了常用的下标法外，还可以用指针操作代替。

例如，如果定义：
```c++
int iArray[10],*iPointer=iArray; 
//或者int *iPointer=&iArray[0]; 
```
这样通过指针iPointer也可以访问数组中的任意一个元素，即：
`iArray[n]`等同于`*(iPointer+n)`

同样，也可以把指针作为数组名使用，使用下标法访问数组元素，以下4句是等价的：
```c++
	iValue = iArray[5];
	iValue = iPointer[5];
	iValue = *(iPointer+5);
	iValue = *(iArray+5);
```
尽管指针和数组名都是指针，但它们也有区别：
指针变量是变量，是可以不断赋值的；而数组名则是指针型常量，只能指向固定的内存地址（即系统为数组所分配的内存空间的首地址），因此不能给数组名赋值，如：
```c++
char cArray[10],ch;
cArray = &ch; //错误，不允许将一个指针值赋给数组名！
```

## 使用指针访问数组元素
如果定义一维数组`int a[10];`则一维数组a中的任何一个元素`a[i]`等同于`*(a+i)`

```c++
//如何将中间的while循环，改为for循环
for(int i=0;i<n;i++,iPointer++)
	iSum+=*iPointer;
//或者：
for(int i=0;i<n;i++)
	iSum+=*iPointer++;
```

```c++
使用指针求一维数组中所有元素之和。
	int sum(int *iPointer,int n)
{
	int iSum=0;
	while(n>0)	{
		iSum+=*iPointer;		
iPointer++;
		n--;
	}
	return iSum;
}
	int main()
	{		int iArray[10]={6,7,8,9,5,4,3,2,10,1};
		 cout<<"数组各元素和: sum="<<sum(iArray,10)<<endl;
		 return 0;
	}
```

使用指针对数组中各元素进行排序。

```c++
void order(int *iPointer,int n)
	{
		   int i,j,t,p;
		   for(i=0;i<n-1;i++)
   {
			 p=i;
			 for(j=i+1;j<n;j++)			
 {					   if(*(iPointer+p)<=*(iPointer+j)) p=j;			}
			 if(p!=i)
			 {
		t=*(iPointer+i);
	*(iPointer+i)=*(iPointer+p);
				*(iPointer+p)=t;
			 }
		   }
	}
	int main(){
		int iArray[10]={6,7,8,9,5,4,3,2,10,1};
		order(iArray,10);
		cout<<"排序后的数组为："<<endl;
		for(int i=0;i<10;i++)
	  		cout<<iArray[i]<<"  ";
		cout<<endl;
		return 0;
	}
```

order函数的其它实现方法：下标法访问数组元素
```c++
void order(int iPointer[],int n)
{
	int i,j,t,p;
	for(i=0;i<n-1;i++)
	{
		p=i;
		for(j=i+1;j<n;j++)
		{
		   		if(iPointer[p]<=iPointer[j]) 
     p=j;
		}
		if(p!=i)
		{
			t=iPointer[i];
			iPointer[i]=iPointer[p];
			iPointer[p]=t;
		}
	}
}
```

## 指向多维数组的指针

多维数组的元素在计算机中也是线性存储的(C++采用按行顺序保存的原则)，只要知道保存某个多维数组的内存区域的首地址，就可以通过指针访问多维数组中的任何一个元素。例如，声明以下二维数组：			
```c++	
int iMatrix [3][4];
```

现在定义一个指针变量，并将该二维数组的首地址赋予它：			
```c++
int *iPointer=&iMatrix[0][0];
```
则二维数组iMatrix中的任何一个元素使用iPointer这个指针来访问的规则是：		
```C++
*(iPointer+m*4+n)		//等同于iMatrix [m][n]
```

多维数组的数组名同样也是指针，但它不指向多维数组的第一个元素，而是指向存放第一个数组元素地址的指针，即多维数组的数组名是指向（指向第一个数组元素的）指针的指针。
比如：将二维数组iMatrix[3][4]看成由3个一维数组```iMatrix[i] (0<i<3)```组成的数组，其中：
```c++
	iMatrix[0]等同于&iMatrix[0][0]
	iMatrix[1]等同于&iMatrix[1][0]
	iMatrix[2]等同于&iMatrix[2][0]
```
因而，有二维数组`iMatrix`就是指向这3个一维数组`iMatrix[i]`组成的数组的第一个元素`iMatrix[0]`的指针，而`iMatrix[0]`是指向`iMatrix[0][0]`的指针。
故：`iMatrix`的数据类型是`int**`，即指向`int`指针变量的指针，也称二级指针
从地址值的角度看，`iMatrix`、`iMatrix[0]`和`&iMatrix[0][0]`所代表的值是相同的，但其本质含义却不同。

```c++
//使用指针实现二维数组转置（行列互换），并输出结果。
#include<iostream> 
#include<iomanip> 
using namespace std;
int main(){
int iMatrix[3][4]={1,2,3,4,5,6,7,8,9,10,11,12};
	int i,j,*iP;
	cout<<"转置前的矩阵："<<endl;
	for(i=0;i<3;i++)
	{
		iP=iMatrix[i];  	//iP存放各行的起始地址(在内循环外)
		for(j=0;j<4;j++)				    cout<<setw(6)<<*(iP+j);
		cout<<endl;
	}
	cout<<"转置后的矩阵："<<endl;
	for(j=0;j<4;j++)	{
		for(i=0;i<3;i++)		{
			iP=iMatrix[i];  //iP存放各行的起始地址(在内循环中)
			cout<<setw(6)<<*(iP+j);
		}
		cout<<endl;
	}
	return 0;
}
```

## 多级指针

多级指针一般又称为指针的指针。如果指针变量保存的是另一个指针变量的地址，则这个指针变量就是指针的指针。定义二级指针变量的一般形式是：		
`<类型标识符> **二级指针变量名`
例如：
```c++
int i=5,*p=&i,**pp=&p;
```

使用指针操作二维数组，并输出相应的指针值（地址）。
注意：比较各级指针加1后，地址实际增加的量。

## 指针数组

如果数组的每一个元素都是类型相同的指针变量，则该数组称为指针数组。声明指针变量数组的形式如下：
```
<类型标识符> *指针数组名[数组长度]
```
例如：
```c++
char *pString[6];  //声明一个保存6个char *型元素的指针数组
```

```c++
//今天星期几？
	int i;
	char *pDay[]={"Monday","Tuesday","Wednesday","Thursday",
			"Friday","Saturday","Sunday"};
	char **ppDay;
	cout<<"输入整数1~7:";
	cin>>i;
	ppDay=pDay+i-1;
	cout<<"Today is "<<*ppDay<<endl;
```

## 指针变量作为函数参数

普通变量作为函数参数实现的是数值传递，而指针变量作为函数参数实现的是地址传递。

1. 数值传递

数值传递是普通变量作为函数形参，主调函数中的实参内容是一个数值确定的表达式，适合少量数据的传递。
```c++
void change(int,int);			//函数声明语句
int main(){
	int a=5,b=10;
	cout<<"函数调用前：a="<<a<<" ,b="<<b<<endl;
	 change(a,b); 			 //以a、b的值为实参调用函数
	cout<<"函数调用后：a="<<a<<" ,b="<<b<<endl;
	return 0;
}
void change(int m,int n){
	int temp;	temp=m;	m=n;	n=temp;
	cout<<"函数中参数：m="<<m<<",n="<<n<<endl;
}
```

2. 地址传递
	
地址传递是指针变量作为函数参数,这时主调函数中的实参是某个变量的内存地址，被调函数的形参则必须是与实参类型相同的指针变量，用以接受由实参传过来的地址。函数调用时，通过地址传递的实参与形参指向相同的内存单元，即形参所指内容与实参所指内容为同一段内存空间，因此形参所指内容发生更改，实参也会发生相应的更改。

从键盘输入两个整数，使用地址传递实现两数的真正交换。
```c++
	void change(int *,int *);			//函数声明语句
int main()
{
	int a=5,b=10;
	cout<<"函数调用前：a="<<a<<" ,b="<<b<<endl;
	change(&a,&b); 			 //以a、b值的地址为实参调用函数
	cout<<"函数调用后：a="<<a<<" ,b="<<b<<endl;
	return 0;
}
void change(int *m,int *n)
{
		int temp;		
   temp=*m;		*m=*n;		*n=temp;
		cout<<"函数中参数：m="<<*m<<",n="<<*n<<endl;
}
```
在C++中，需要返回多个变量值时，也可以采用地址传递的方式，将变量地址传给函数形参，在函数内通过指针直接修改位于函数外部的变量的值，从而实现返回多个值的目的。

## 指向函数的指针

要调用一个函数，只要知道被调用函数的入口地址即可。函数名实际上就是一个指针，它指向函数的入口地址。因此可以定义一个指针变量，并赋予它函数名，这样的指针变量就指向该函数的入口地址。称这样的指针变量为指向函数的指针变量，简称函数指针变量，通过函数指针变量就可以实现调用它所指函数的目的。
1．函数指针变量的声明与使用
函数指针变量声明的一般形式为：
```c++
		<类型标识符> (*函数指针变量名)(参数表)
```
注意：		
类型标识符说明函数指针变量所指函数返回值的数据类型；		
定义中的(*函数指针变量名)的两对圆括号都不能遗漏，否则就成了返回指针的函数；			
参数表中形参的类型、个数都必须与所指的函数一致。					

若定义函数：`void func(int m,int n);`		
则指向该函数的函数指针变量可定义为：		
` void (*pFpointer) (int,int);`			
使用已经定义的函数指针变量来调用函数，分以下两步：		
（1）将函数名赋予己定义的函数指针变量，其形式为：		
```
函数指针变量名=函数名
```
如：`pFpointer = func;`			
（2）使用函数指针变量调用它所指的函数，可用的形式有：		
```
(*函数指针变量名)(实参表)
或：		
函数指针变量名(实参表)
```
如，下面三个函数调用语句的功能是相同的：
```c++
func(3,4);
(* pFpointer)(3,4);
pFpointer(3,4);
```
注意：函数的指针不能进行算术运算,因此不能：
```c++
pFpointer++ //错误！
pFpointer-1 //错误！
```
函数指针变量作为函数的参数
函数指针的用处在于可以作为函数参数传递给被调函数，使得被调函数可以根据情况，调用不同的的函数，增强其通用性。

将函数指针变量作为函数参数。
```c++	
int add(int x,int y)	{		return x+y;	}
int sub(int x,int y)	{		return x-y;	}
int funct(int (*pFunc)(int,int),int x,int y)
{
	int result;
	result=(*pFunc)(x,y); //等价于result=pFunc(x,y);
	return result;
}
int main()	{
int a,b;		
cin>>a>>b;
cout<<"a+b="<<funct(add,a,b)<<endl;
cout<<"a-b="<<funct(sub,a,b)<<endl;
return 0;
}
```

## 用梯形积分公式求两个函数的积分

```c++
#include<iostream>
#include<iomanip>
using namespace std;
float f1(float x);//函数声明
float f2(float x);//函数声明
float integral(float (*p)(float),float a,float b);//函数声明
int main()
{
   float s1,s2;
   s1=integral(f1,1,1.1);//函数调用
   s2=integral(f2,1,1.1);//函数调用
   cout<<"s1="<<setprecision(5)<<s1<<endl;
   cout<<"s2="<<setprecision(5)<<s2<<endl;
   system("pause");
   return(0);
} 
float integral(float (*p)(float),float a,float b)
{
   float sum;
   sum=0.5*(b-a)*(p(a)+p(b));//利用函数指针间接调用函数
   return(sum);
}
float f1(float x)
{
   float s;
   s=2*x+3;
   return(s);
}
float f2(float x)
{
   float s;
   s=x*x-3*x+5;
   return(s);
}
```

## 指针作为函数的返回类型

返回值是地址的函数称为指针函数，其定义的一般形式为：
```
<类型标识符>*函数名(形参表)
{  函数体 }
```

```c++
//函数的返回值是指针类型。
#include<iostream>
using namespace std;
int *pfun(int *p){ 
	if(*p!=0) 	{
		cout<<p+1<<endl; 		return p+1;
	}
	else 		return p;
}
int main(){
	int s[]={1,2,3};
	cout<<*pfun(&s[1])<<endl; //	cout<<pfun(&s[1])<<endl;
	system("pause");
	return 0;
}
```
注意：作为函数的返回的指针，不能是在函数体内定义的局部变量。	

## 字符型指针与字符串

对字符串的再认识:				
字符串由一系列字符组成，每一个字符在内存中按顺序存放，最后一个字符后面一定是结束符’\0’ 。			

在内存中识别字符串关键有以下两点：					
①字符串的首地址。				
②字符串结束符’\0’。				
对字符串的处理通常是采用循环语句，从字符串的首地址开始，依次取出指针所指内存里的各个字符，直至遇到’\0’为止。				

## 使用字符型指针定义字符串

使用字符型指针定义字符串的形式如下：
```
	char  *指针变量名="字符串"
```
注意：字符型指针比字符型数组更灵活（因为字符型数组只能初始化，不能再次赋值，而字符型指针定义字符串却可以！）。
```c++
	char pString1[]="I love China!";
	char *pString2="This is a string2.";
		pString2="This is a new string2."; 	//允许，且新字符串更长！
	pString1="This is a new string1."; 	//不允许，数组名不能再赋值pString2=pString1;			//允许
	pString1=pString2;			//不允许，数组名不能被赋值
```
但是：
```c++
*pString1 = 'a';//允许, pString1[0]= 'a'
*pString2 = 'a';//不允许，使用字符型指针定义字符串后，指针所指单个内存的内容只允许读，不允许写！
*pString1 = *pString2;//允许, pString1[0]= pString2[0]=‘T'
```

使用字符型指针实现字符串的复制。
```c++
void copy_string(char *to,char *from)//复制函数
{
	for(;*from!='\0';from++,to++)
			     *to=*from;
	*to='\0';  //赋值字符串结束标识
}
int main()	{
	char pSource[]="I am a teacher.";//可以用char *pSource替换
	char pDestination[]="you are a student.";
	//不能用char *pDestination替换（使用字符型指针定义字符串后，指针所指单个内存的内容只允许读，不允许写！ ）
	//pDestination字符数组长度>=pSource字符数组长度
	copy_string(pDestination,pSource);
	cout<<"pSource:"<<pSource<<endl;
	cout<<"pDestination:"<<pDestination<<endl;
	return 0;
} 
```

## 字符串标准库函数

C++提供了许多操作字符串的标准库函数，这些函数使用时，必须在应用程序的开头添加包含头文件string.h的预处理命令：	`#include <string>`

```c++
//使用C++的标准库函数strcpy( )实现字符串的复制。
	
#include<iostream> 
#include <string>
using namespace std;
int main()
	{
		char pSource[]="I am a teacher."; //15个字符
		char pDestination[]="you are a student."; //18个字符
			//若char pDestination[]=“student.”; 后面拷贝时会错误！
//pDestination字符串长度>=pSource字符串长度
		strcpy(pDestination,pSource);
			//等价于strncpy(pDestination,pSource,16);
		cout<<"pSource:"<<pSource<<endl;
		cout<<"pDestination:"<<pDestination<<endl;
		return 0;
	}
```

## 动态内存分配与new和delete运算符

静态内存分配是指在编译阶段就分配好存储空间，其空间大小在程序运行过程中不可更改；动态内存分配则是在程序中通过调用内存分配函数malloc或内存分配运算符new取得存储空间；				
区别：静态内存分配的变量空间，在其生存期结束后自动收回；而动态内存分配的存储空间，必须由程序员通过语句显式地将其归还给系统。				

## new运算符

new运算符用于在内存中动态分配一块存储空间。new运算符的使用形式为：
```
指针变量=new  <类型标识符>[长度]
```
例如：	
```c++
char *cBuffer=new char [256]; 			//分配256个char型数据的存储空间
```
注意：				

①如果分配的存储空间长度为1个单位，则可以省略new运算符格式中的中括号和中括号中的整数。
  如：int *pInt=new int; //等价于int *pInt=new int[1];			
	
②使用new运算符分配存储空间时，其空间长度可以是变量，也可以是数值表达式，但无论是什么，其类型必须是整型。			
```c++
	int nSize=5; int *pInt=new int[nSize+5];
```
③由new所分配的存储空间是连续的，且通常将存储空间的首地址赋予一个指针变量，所以可以通过该指针的变化访问所分配存储空间的每一个元素。						

④如果当前内存无足够的存储空间可分配，则new运算符返回NULL。			

由new运算符分配的内存空间在使用完毕之后必须使用delete运算符释放。					
delete运算符的使用有两种形式：					
形式一：			
```c++
delete指针	//释放空间长度为1个单位（与指针类型有关）的内存空间
```
形式二：			
```c++
delete [ ]指针	//释放空间长度大于1的内存空间
```
delete后面的是使用new运算符分配内存空间时返回的指针	，也可以是NULL，若是NULL，则delete运算符什么也不做。			
例如：			
```c++
int *pInt=new int;
delete pInt;
int *pManyInt = new int[10];
delete []pManyInt; 
```
注意：				
①用new运算符获得的内存空间，只许使用一次delete，不允许多次对同一块空间进行多次释放，否则将会产生严重错误。							
②delete只能用来释放由new运算符分配的动态内存空间，对于程序中的变量、数组的存储空间，不得使用delete运算符释放。					

复制字符串，依据源字符串的长度动态分配目的字符串的存储空间。					
```c++
#include <iostream>
using namespace std;
void copy_string(char *to, char *from)//复制字符串函数
{
char * tt=to;
while(*from!='\0’) 
         *tt++=*from++;
*tt='\0';//赋值字符串结束标识
} 
int main()
{
      char pSource[]="I am a teacher.";
int nSize=0;
while(pSource[nSize]!=0)
   nSize++;
//定义目的字符串并申请分配内存空间(源字符串长度并加1)
char *pDestination=new char[nSize+1];
if(pDestination!=NULL)
{
copy_string(pDestination,pSource);
cout<<"pSource:"<<pSource<<endl;
cout<<"pDestination:"<<pDestination<<endl;
}
else
       cout<<"No memory space to copy operation!"<<endl;

delete []pDestination;

system("pause");
return 0;
}
```


































