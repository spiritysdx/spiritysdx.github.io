# Python数据分析快速入门

# 前言

本篇鸣谢 燕大——马川 的整理

匹配Jupyter Notebook的ipynb文档链接下载地址如下

[**源文档**](https://spiritlhl.lanzous.com/icfrrsj)

# Python编程基础

>*by* 马川  *燕大*

>**代码胜于雄辩**  
>**Talks is cheap. Show me the code.**  
>                      ----Linus Torvalds(Linux操作系统的奠基者)



对于任何一种计算机语言，最重要的就是：

1. **<font color='red'>数据类型</font>**

2. **<font color='red'>控制结构</font>**

3. **<font color='red'>函数</font>**

这三方面一定要打牢基础。

此外 Python 非常简洁，具有很多**<font color='red'>高级特性</font>**，使得一行代码 (one-liner) 就能做很多事情，我们将重点介绍各种「解析式」和「高阶函数」。 

shirft + Tab 看Jupyter Notebook解释文档    

## 00 Python概览

![Mcpicture](https://pic./2020/05/09/02d80036680b6.png)

#直接在当前文件里运行时会调用main，但是在其他文件里import这个文件，就不会执行这个文件的main（上图解释）

### Python程序实例解析

#### 温度转换程序

根据华氏和摄氏温度定义，转换公式如下：

          C = ( F – 32 ) / 1.8
          
          F = C * 1.8 + 32
          
其中，C表示摄氏温度，F表示华氏温度

```python
#e1.1TempConvert.py
TempStr = input("请输入带有符号的温度值: ")
if TempStr[-1] in ['F','f']:
    C = (eval(TempStr[0:-1]) - 32)/1.8
    print("转换后的温度是{:.2f}C".format(C))
elif TempStr[-1] in ['C','c']:
    F = 1.8*eval(TempStr[0:-1]) + 32
    print("转换后的温度是{:.2f}F".format(F))
else:
    print("输入格式错误")
```

### Python语法元素分析

格式框架、注释、变量、表达式、语句函数

### 缩进

1个缩进 = 4个空格

(1)用以在Python中标明代码的层次关系

(2)缩进是Python语言中表明程序框架的唯一手段

### 注释

单行注释以#开头  

```python
print("Hello world")  #打印显示
```

多行注释以 '''开头和结尾

'''python
  This is a multiline comment
  used in Python
'''

### 命名与保留字

**常量**： 程序中值不发生改变的元素

**变量**：程序中值发生改变或者可以发生改变的元素

Python语言允许采用**大写字母、小写字母、数字、下划线(_)和汉字等字符及其组合**给变量命名，但名字的**首字符不能是数字**，**中间不能出现空格**，长度没有限制   
注意：标识符对**大小写敏感**，python和Python是两个不同的名字 

### 字符串

Python语言中，字符串是用两个双引号“ ”或者单引号‘ ’括起来的一个或多个字符。

Python字符串的两种序号体系

![Mcpicture](https://pic./2020/05/09/d2ba4b2bbe9aa.png)

```python
str = "Hello World"
print(str[0:-1])
```

### 赋值语句

Python语言中，= 表示“赋值”，即将等号右侧的值计算后将结果值赋给左侧变量，包含等号（=）的语句称为“赋值语句”  

#### 同步赋值

```python
x=5
y=10
x,y=y,x
print(x,y)

x,y,z=1,4,5
print(x,y,z)
```

### input()函数

获得用户输入之前，input()函数可以包含一些提示性文字 

                <变量> = input(<提示性文字>)  字符型

```python
input("请输入: ")
```

### eval() 函数

eval(<字符串>)函数是Python语言中一个十分重要的函数，它**去掉字符串最外侧的引号**并以**Python表达式的方式解析并执行**去掉引号后的字符串内容，将返回结果输出(会进行数值运算)

```python
tmp="102C"
print(eval("tmp"))

print(eval(tmp[0:-1]))

value=eval(input("输入数值"))
print(value*2)

s = "11+5in"
print(eval(s[1:-2]))
```

### 输出函数

```python
#print()函数用来输出字符信息，或以字符形式输出变量。
F=10.258
print(F)
print("转换后的温度是{:.2f}".format(F)) 
```

### Python 库的引入与调用

### import关键字

import是一个关键字，用来引入一些外部库

#### 引入方式1

```python
import numpy
print(numpy.arange(0,10))
```

#### 引入方式2

```python
from numpy import *   
print(arange(0,10))
```
#### 引入方式3

```python
import numpy as np

print(np.arange(0,10))

```
## 01 数据

在 Python 中数据可分两大类：

* **基本数据类型**(元素型)：整数、浮点数、复数、布尔型

* **组合数据类型**(容器型)：字符串、元组、列表、字典、集合

### 1.1 基本数据类型（元素型）

Python 里面有自己的内置数据类型 (build-in data type)，本节介绍四种**数字类型(Numbers)**:

1. **整型 (int)**

2. **浮点型 (float)**

3. **复数 (complex)**

4. **布尔型 (bool)**

#### 思维导图(图中左侧部分的字符串属于组合数据类型)

![Mcpicture](https://pic./2020/05/09/36b56fea9ab37.png)

### 数字类型

```python
print( 1, type(1) )
print( 1., type(1.) )
print( 1 + 2j, type(1 + 2j) )
print( True, type( True ))

```

#### 布尔型 (boolean)

布尔 (boolean) 型变量只能取两个值，True 和 False。当把布尔变量用在数字运算中，用 1 和 0 代表 True 和 False。

```python
T = True
F = False
print( T + 2 )
print( F - 8 )

```

### 数字类型的转换

```python
int(4.5) # = 4  ，直接去掉小数部分

float(4) # = 4.0，增加小数部分

complex(4) # = 4 + 0j

bool(4.5)

```
**bool(x)** 

若x为基本数据类型，则只要x不是整型 0、浮点型 0.0，bool(x) 就是 True，其余就是 False。

若x为组合数据类型，则只要x不是空的变量，bool(x) 就是 True，其余就是 False。

### 类型的判断

**type()**

函数：type(x)，返回x的类型，适用于所有类型的判断

```python
print(type(4+2j))
```
### dir和help函数

**dir()**

dir()用来查询一个类或者对象的所有属性

```python
dir(list)
```

**help()**

help()函数帮助我们了解模块、类型、对象、方法、属性的详细信息

1.帮助查看类型详细信息，包含类的创建方式、属性、方法

```python
help(list)
```
2.帮助查看方法的详细使用信息（使用时要注意输入完整路径，使用模块帮助时，需要先导入模块）

```python
import math
help(math.sqrt)
```
## 字符串类型及其操作

字符串是用双引号“”或者单引号‘’ 或’’’括起来的一个或多个字符。

字符串可以保存在变量中，也可以单独存在。

可以用type()函数测试一个字符串的类型

```python
print('单引号表示可以使用"双引号"作为内容')
print("双引号表示可以使用'单引号'作为内容")
print('''三引号表示可以使用"双引号"
             单引号'作为内容'，还可以换行
      ''')
```
### 常用的字符串处理函数

### len

len()函数返回一个字符串的长度

```python
print(len("天行健，君子以自强不息"))
```

### str

大多数数据类型都可以通过**str()函数转换为字符串**

```python
str(123.456)
str(123e+10)
```
### 遍历字符串

可以通过 for 和 in 组成的循环来遍历字符串中每个字符

```python
mystr="地势坤，君子以厚德载物"
for s in mystr:
    print(s)

```
### split

按指定字符分割字符串为数组

```python
mystr="敕勒川，阴山下，天似穹庐，笼盖四野"
print(mystr.split("，"))
```
### join

**连接两个字符串序列**

```python
mystr = "@"
ls = ["天苍苍","野茫茫","风吹草低见牛羊"]
print(mystr.join(ls))
```
### replace

字符串替换

```python
myOldStr="今夕何夕溪，搴舟中流。今日何日溪，得与王子同舟。"
myNewStr=myOldStr.replace("溪","兮")
print(myNewStr)
```
### format方法的基本使用

```python
hero="乔峰"
department="丐帮"
skill="降龙十八掌"

print("{}大侠，{}人士，成名绝技{},此前已刻苦练功{}个时辰".format(hero,department,skill,1234.5678))

#"{0:*^30}大侠，{1}人士，成名绝技{2},此前已刻苦练功{3:,.2f}个时辰".format(hero,department,skill,1234.5678)

```

### 1.2 组合数据类型(容器型)

### 思维导图

![Mcpicture](https://pic./2020/05/09/3e15538fcb5ec.png)

## 序列

#### 序列是一组有顺序的元素向量，通过序号访问，元素之间不排他。

### 1. 字符串(具体细节见“基本数据类型”思维导图左侧部分)

```python
mystr="Hello world"
mystr[0:3]
mystr[0:-1]
print("Hello world"[0:3])
```
### 2. 元组

```python
creature = "cat", "dog", "tiger", "human"


color = ("red", 0x001100, "blue", creature)
color[2]
color[-1][2]#索引可以索引两重
```
#### 应用场景

```python
def func(x):  #函数多返回值
    return x, x**3

a, b = 'dog', 'tiger'  #多变量同步赋值
a, b = (b, a)      #多变量同步赋值，括号可省略

func(3)
```

```python
import math
for x, y in ((1,0), (2,5), (3,8)):  #循环遍历
        print(math.hypot(x,y))            #求多个坐标值到原点的距离

```

### 3. 列表 

```python
ls = [425, "BIT", [10, "CS"], 425]
ls[2][-1][0]#多重索引

list((425, "BIT", [10, "CS"], 425))

list("中国是一个伟大的国家")

list()

print(ls[2][-1][0])
```

```python
ls = [425, "BIT", 1024]  #用数据赋值产生列表ls
lt = ls       #lt是ls所对应数据的引用，lt并不包含真实数据
#lt = ls[:]    #ls通过分片操作将列表ls的元素全部拷贝给lt
ls[0] = 0     
print(id(ls),id(lt))
```

```python
vlist = list(range(5))

len(vlist[2:])      #计算从第3个位置开始到结尾的子串长度

2 in vlist           #判断2是否在列表vlist中

vlist[3]="python"   #修改序号3的元素值和类型

vlist[1:3]=["bit", "computer"] 

print(vlist)
```
#### 多增少减

```python
vlist[1:3]=["new_bit", "new_computer", 123]
vlist

#vlist[1:3]=["fewer"]
print(vlist)
```

```python
#以k为步数
ls = [425, "BIT", [10, "CS"], 123, "Hello Ysu", 23 , (10,29)]
lt=["1st","2nd","3rd"]

ls[0:5:2] = lt
print(ls)

del ls[0:5:2]#删去了024号元素
print(ls)

```

```python
ls = ["1st","2nd","3rd"]
lt = [425, "BIT", [10, "CS"], 425, "Hello Ysu", 23, 425, (10,29)]

#在列表ls最后增加一个元素x
ls.append("4th")
print(ls)

#删除ls中所有元素
ls.clear()
print(ls)

#生成一个新列表，复制lt中所有元素
ls = lt.copy()
print(ls)

#在列表ls第i位置增加元素x
ls.insert(1,"ysu")
print(ls)

#将列表ls中第i项元素取出并删除该元素BIT
ls.pop(2)

#将列表中出现的第一个元素x删除
ls.remove(425)
print(ls)

#列表ls中元素反转
ls.reverse()
print(ls)

```

```python
for e in vlist:
    print(e, end=" ")#不换行显示
```
### 4. 集合

#### 集合类型是一个元素集合，元素之间无序，相同元素在集合中唯一存在。

```python
#元素类型只能是固定数据类型，例如：整数、浮点数、字符串、元组等
S = {425, "BIT", (10, "CS"), 424}

T = {425, "BIT", (10, "CS"), 424, 425, "BIT"}

#列表、字典和集合类型本身都是可变数据类型，不能作为集合的元素出现。
X = {425, "BIT", [10, "CS"], {"蜀":"诸葛亮"}, {234,(10,"haha")}}
```

```python
#set(x)函数可以用于生成集合
W = set('apple')

V = set(("cat", "dog", "tiger", "human"))

print(V)
```

#### 应用场景

```python
"BIT" in {"PYTHON", "BIT", 123, "GOOD"} #成员关系测试

tup = ("PYTHON", "BIT", 123, "GOOD", 123)

tup1 = set(tup)#元素去重

newtup = tuple(set(tup)-{'PYTHON'}) # 去重同时删除数据项

print(tup1)
```

#### 集合类型的4种基本操作，交集（&）、并集（|）、差集（-）、补集（^），操作逻辑与数学定义相同

![Mcpicture](https://pic./2020/05/09/817266094e8e1.png)

```python
A = {425, "BIT", (10, "CS"), 424, 125, "This is A"}

B = {425, "BIT", (10, "CS"), 424, 425, "BIT","This is B"}

print(A | B)

print(A - B)

print(A & B)

print(A ^ B)

```

```python
s = [123,(45,"Hello world"),"set"]

t = [123,(45,"Hello world"),"set","haha"]

print(s <= t)

print(s >= t)

```

### 5. 映射
#### 映射类型是“键-值”数据项的组合，每个元素是一个键值对，表示为(key, value)
#### 字典是集合类型的延续，各个元素并没有顺序之分
```python
Dcountry={"中国":"北京", "美国":"华盛顿", "法国":"巴黎"}
print(Dcountry)

#访问
Dcountry["中国"]

#修改
Dcountry["中国"]='大北京'
print(Dcountry)

#增加新元素
Dcountry={"中国":"北京", "美国":"华盛顿", "法国":"巴黎"}
Dcountry["英国"]="伦敦"
print(Dcountry)

#直接使用大括号（{}）可以创建一个空的字典，并通过中括号（[]）向其增加元素
Dp={}
Dp['2^10']=1024
print(Dp)
```
#### 字典类型的操作
```python
Dcountry={"中国":"北京", "美国":"华盛顿", "法国":"巴黎"}
Dcountry.keys()

list(Dcountry.values())

Dcountry.items()

'中国' in Dcountry   #只对键进行判断

Dcountry.get('美国', '悉尼') #'美国'在字典中存在

Dcountry.get('澳大利亚', '悉尼') #'澳大利亚'在字典中不存在

for key in Dcountry:
    print(key)

```
## 02 程序的控制结构
### 思维导图
![Mcpicture](https://pic./2020/05/09/32b427223adab.png)

### 分支结构
```python
#求两个数的最大值
x=int(input("请输入x:"))
y=int(input("请输入y:"))

if x>y:
    print(x)
else:
    print(y)
```
```python
#条件判断从左到右执行，并且在and或or两侧的条件会有"短路"现象
a = 5
b = 7
c = 8
d = 6
if a<b or c>d:
    print("True")

def suma(y):
    global a 
    a=a+y
    return a
if suma(2) or suma(3):
    print("a={}".format(a))

```
#### 身体质量指数BMI
![Mcpicture](https://pic./2020/05/09/afaa0fcbdb3d0.png)
```python
#多分支
height, weight = eval(input("请输入身高(米)和体重(公斤)[逗号隔开]: "))
bmi = weight / pow(height, 2)
print("BMI数值为：{:.2f}".format(bmi))
wto, dom = "", ""
if bmi < 18.5:
    wto, dom = "偏瘦", "偏瘦"
elif 18.5 <= bmi < 24:
    wto, dom = "正常", "正常"
elif 24 <= bmi < 25:
    wto, dom = "正常", "偏胖"
elif 25 <= bmi < 28:
    wto, dom = "偏胖", "偏胖"
elif 28 <= bmi < 30:
    wto, dom = "偏胖", "肥胖"
else:
    wto, dom = "肥胖", "肥胖"
print("BMI指标为:国际'{0}', 国内'{1}'".format(wto, dom))
```
### 循环结构

#### 遍历循环：for语句

循环次数确定，循环次数采用遍历结构中元素的个数来体现

```python
#判断一个数是否为素数
integer=int(input("请输入一个整数:"))
flag=False
for i in range(2,integer):#从2到输入的整数求余数，若都为0就是素数
    if integer%i==0:
        break
else:     # if i==integer-1
    flag=True

print("问：整数{}是素数吗？\n答：{}".format(integer,flag))

```
**continue** vs **break**

```python
for i in range(4):
    for j in range(4):
        if j==i:         
            print('-',end='\t')
            break       # 试试continue
        print('*',end='\t')
    print()
```
#### 无限循环：while语句

循环次数不确定。无限循环一直保持循环操作，直到特定循环条件不被满足才结束

```python
#猜密码
guess=0     #输入的数字  
secret=7    #预设的数字

while guess!=secret:    #条件  也可用True/barek构造无限循环
    guess=int(input("@数字区间0-9，请输入你猜的数字:"))  
    if guess==secret:  
        print("你猜对了，真厉害！")
    else:
        print("很遗憾，猜错了！")
print("游戏结束") 
```

```python
#猜密码 
secret=7    #预设的数字

while True:    # 用True/barek构造无限循环
    guess=int(input("@数字区间0-9，请输入你猜的数字:"))  
    if guess==secret:  
        print("你猜对了，真厉害！")
        break
    else:
        print("很遗憾，猜错了！")
print("游戏结束") 

```
## 03 函数
### 思维导图
![Mcpicture](https://pic./2020/05/09/825e58187775e.png)

![](https://pic./2020/05/09/e39b8d0e9bead.png)

* def - 使用def关键字定义函数

* function_name - 函数名，起名应有意义，见名知意

* arg1 - 位置参数 ，这些参数在调用函数 (call function) 时位置要固定

* arg2 = v - 默认参数 = 默认值，调用函数的时候，默认参数已经有值，可省略

* *args - 可变参数，可以是从零个到任意个，自动组装成元组

* ：- 冒号，在第一行最后要加个冒号，表示后面内容为函数体部分

* """docstring""" - 函数说明，用于介绍该函数，可省略，但写函数说明是一个好习惯，可使你写的代码可读性更好

* statement - 函数体部分(函数内容)

### 函数的参数传递
#### 默认值
```python
def dup(str, times = 2):
    print(str*times)

dup("knock~")
dup("knock~",4)

```
#### 可选参数
```python
def func(x1,y1,z1,x2,y2,z2):
    return (x1,y1,z1,x2,y2,z2)

# 按位置给参数赋值
result = func(1,2,3,4,5,6)

# 按参数名给参数赋值
result = func(x2=4, y2=5, z2=6, x1=1, y1=2, z1=3)

```
#### 可变参数
```python
def vfunc(a, *b):
    print(type(b))
    for n in b:
        a += n
    return a

vfunc(1,2,3,4,5)

```
**注意：**

在 Python 中定义函数时，若定义了位置参数、默认参数、可变参数，参数定义的顺序必须是：

**<font color='red'>位置参数、默认参数、可变参数</font>**

否则，程序会报错。

#### 函数的返回值

```python
def func(a, b):
    return a*b
    
s = func("knock~", 2)
print(s)

def func(a, b):
    return b,a
s = func("knock~", 2)
#print(s, type(s))
```
#### 全局变量与局部变量

```python
n = 1    #n是全局变量
def func(a, b):
    c = a * b     #c是局部变量，a和b作为函数参数也是局部变量
    return c
s = func("knock~", 2)
print(c)
print(s)
```
```python
n = 1    #n是全局变量
def func(a, b):
    n = b     #这个n是在函数内存中新生成的局部变量        return a*b
s = func("knock~", 2)
print(s, n)

```
```python
n = 1    #n是全局变量
def func(a, b):
    global n
    n = b     #将局部变量b赋值给全局变量n 
    return a*b
s = func("knock~", 2)
print(s, n)

```
```python
ls = []    #建立ls全局列表变量
def func(a, b):
    ls.append(b)   #将局部变量b增加到全局列表变量ls中 
    return a*b
s = func("knock~", 2)
print(s, ls)  #测试一下ls值是否改变


```
```python
ls = []    #ls是全局列表变量
def func(a, b):
    ls = []     #创建了名称为ls的局部列表变量列
    ls.append(b)   #将局部变量b增加到局部列表变量ls中 
    return a*b
s = func("knock~", 3)
print(s, ls)  #测试一下ls值是否改变


```
### 函数的递归
```python
def fact(n):
    if n == 0:
        return 1
    else:
        return n * fact(n-1)
num = eval(input("请输入一个整数: "))
print(fact(abs(int(num))))

```

```python
def reverse(s):
    if s=="":
       return s
    else:
       return reverse(s[1:]) + s[0]


```
#### lambda函数(匿名函数)

![](https://pic./2020/05/09/875637c1627f4.png)

* lambda - 定义匿名函数的关键字

* argument_list - 函数参数，可以是位置参数、默认参数、可变参数等，和正规函数里的参数类型一样

* ：- 冒号，在函数参数和表达式中间要加个冒号

* expression - 函数表达式，输入函数参数，输出一些值

注意 lambda 函数没有所谓的函数名，所以也叫匿名函数。下面是一些 lambda 函数示例：  

lambda所表示的匿名函数的内容应该是很简单的，如果复杂的话，干脆就重新定义一个函数了，使用lambda就有点过于执拗了。


```python
# 输入 x 和 y，输出其积 x*y
func = lambda x, y: x*y
func(2, 3)

```
和下面的正规函数等价：
```python
def func(x,y):
    return x*y
func(2,3)

```


```python
# 输入任意个数的参数，输出其和
func = lambda *args: sum(args)
func( 1, 2, 3, 4, 5 )

```
和下面的正规函数等价：
```python
def func(*args):
    return sum(args)
func( 1, 2, 3, 4, 5 )
```

## 04 高级特性
在Python中，**<font color='red'>代码不是越多越好，而是越少越好。代码不是越复杂越好，而是越简单越好</font>**。

因此，Python中有许多高级特性，这里仅介绍**推导式**和**高阶函数**。
### 推导式

推导式comprehensions（又称生成式、解析式），是Python的一种独有特性。推导式是可以从一个数据序列构建另一个新的数据序列的结构体。 共有三种推导，在Python2和3中都有支持：

* 列表(list)推导式
* 字典(dict)推导式
* 集合(set)推导式

**基本格式**

variable = [out_exp_res for out_exp in input_list if out_exp == 2]

[要添加的元素 for循环  if判断条件]

  * out_exp_res:　　列表生成元素表达式，可以是有返回值的函数。
  * for out_exp in input_list：　　迭代input_list将out_exp传入out_exp_res表达式中。
  * if out_exp == 2：　　根据条件过滤哪些值可以。
  
**问题：**

如何从一个含整数列表中把奇数 (odd number) 挑出来？  

```python
lst = [1, 2, 3, 4, 5] 
odds = []
for n in lst:
    if n % 2 == 1:
        odds.append(n * 2)
odds

```
任务完成了，但不够简洁，看看下面这一行代码：  
```python
odds = [n * 2 for n in lst if n % 2 == 1]
odds

```
乍一看从「for 循环」到「解析式」不直观，我来用不同颜色把这个过程可视化一下，如下图：
![](https://pic./2020/05/09/75caec28f258f.jpg)

现在你可能会说上面「for 循环」只有一层，如果两层怎么转换「列表解析式」？具体来说怎么解决下面这个问题。  

问题：

如何用「列表解析式」将一个二维列表中的元素按行一个个展平？

没思路？先用「for 循环」试试？

```python
lst = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = []
for row in lst:
    for n in row:
        flattened.append(n)
flattened

```
套用一维「列表解析式」的做法

![](https://pic./2020/05/09/2ba9449877d35.jpg)

两点需要注意：

* 该例没有「if 条件」条件，或者认为有，写成「if True」。如果有「if 条件」那么直接加在「内 for 循环」后面。

* 「外 for 循环」写在「内 for 循环」前面。

```python
lst = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [n for row in lst for n in row]
flattened

```
我们把「列表解析式」那一套举一反三的用到其他解析式上，用下面两图理解一下「字典解析式」和「集合解析式」。

![](https://pic./2020/05/09/f455ed3b02cc5.jpg)
![](https://pic./2020/05/09/48e5bb9ffa695.png)

再看一些例子：

```python
#列表生成式
#两层循环
[m + n for m in 'ABC' for n in 'XYZ']

```
```python
#字典生成式
#大小写key合并
mcase = {'a': 10, 'b': 34, 'A': 7, 'Z': 3}
mcase_frequency = {v: k for k, v in mcase.items()}
print(mcase_frequency)

```
```python
#集合生成式
squared = {x**2 for x in [1, 1, 2]}
print(squared)

```
### 高阶函数

高阶函数 (high-order function) 在函数化编程 (functional programming) 很常见，主要有两种形式：

1. 参数是函数 (map, filter, reduce)

2. 返回值是函数 (closure, partial, currying)

这里只介绍第一种。


#### Map, Filter, Reduce

Python 里面的 map, filter 和 reduce 属于第一种高阶函数，参数是函数。这时候是不是很自然的就想起了 lambda 函数？

**<font color='red'>作为内嵌在别的函数里的参数，lambda 函数就像微信小程序一样，即用即丢，非常轻便。</font>**

首先看看 map, filter 和 reduce 的语法：

**map(函数 f, 序列 x)：**

* 对序列 x 中每个元素依次执行函数 f，将 f(x) 组成一个「map 对象」返回 (可以将其转换成 list 或 set)

**filter(函数 f, 序列 x)：**

* 对序列 x 中每个元素依次执行函数 f，将 f(x) 为 True 的结果组成一个「filter 对象」返回 (可以将其转换成 list 或 set)

f(x)是一个判断函数，取值为正的时候返回对象

**reduce(函数 f, 序列 x)：**

* 对序列 x 的第一个和第二个元素执行函数 f，得到的结果和序列 x 的下一个元素执行函数 f，一直遍历完的序列 x 所有元素。

![](https://pic./2020/05/09/9824258a3644e.jpg)

map() 函数接收两个参数，一个是函数，一个是 Iterable (可迭代对象，如列表、元组、字典、字符串等可以用for遍历的数据结构)，map 将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator(可以将其转换成 list 或 set)返回。

看个具体的平方示例，用 map 函数对列表每个元素平方。

![](https://pic./2020/05/09/47edafb6396ff.png)

```python
lst = [1, 2, 3, 4, 5, 6, 7, 8, 9]
map_iter = map( lambda x: x**2, lst )
print( map_iter )
print( list(map_iter) )

```
接着再看看 filter 函数，顾名思义就是筛选函数，那么我们把刚才列表中的计数筛选出来吧。
```python
filter_iter = filter(lambda n: n % 2 == 1, lst)
print( filter_iter )
print( list(filter_iter) )

```
在 filter 函数中

* 第一个参数是一个识别奇数的「匿名函数」

* 第二个参数是列表，即该「匿名函数」作用的对象

同样，filter_iter 作为 filter 函数的返回对象，也是一个迭代器，想要将其内容显示出来，需要用 list 将其转换成「列表」形式。

最后来看看 reduce 函数，顾名思义就是累积函数，把一组数减少 (reduce) 到一个数。

```python
from functools import reduce
reduce( lambda x,y: x+y, lst )

```
在 reduce 函数中

* 第一个参数是一个求和相邻两个元素的「匿名函数」
* 第二个参数是列表，即该「匿名函数」作用的对象

在 reduce 函数的第三个参数还可以赋予一个初始值

```python
reduce( lambda x,y: x+y, lst, 100 )
```

## 05 综合示例
## Jieba库的使用
```python
#jieba库的安装
#pip install jieba

import jieba

jieba.lcut("中国是一个伟大的国家")

```
#### 精确模式：将句子最精确地切开，适合文本分析
```python
jieba.lcut("中华人民共和国是一个伟大的国家")
```
#### 全模式：把句中所有可以成词的词语都扫描出来，速度快，但不能解决歧义
```python
jieba.lcut("中华人民共和国是一个伟大的国家", cut_all=True)
```
#### 搜索引擎模式：在精确模式的基础上，对长词再次切分，适合搜索引擎分词
```python
jieba.lcut_for_search("中华人民共和国是一个伟大的国家")
```
## 《三国演义》人物出场统计
```python
import jieba
excludes = {"将军","却说","荆州","二人","不可","不能","如此"}
txt = open("三国演义.txt", "r", encoding='utf-8').read()
words  = jieba.lcut(txt)
counts = {}
for word in words:
    if len(word) == 1:
        continue
    elif word == "诸葛亮" or word == "孔明曰":
        rword = "孔明"
    elif word == "关公" or word == "云长":
        rword = "关羽"
    elif word == "玄德" or word == "玄德曰":
        rword = "刘备"
    elif word == "孟德" or word == "丞相":
        rword = "曹操"
    else:
        rword = word
    counts[rword] = counts.get(rword,0) + 1
for word in excludes:
    del(counts[word])
items = list(counts.items())
items.sort(key=lambda x:x[1], reverse=True) 
for i in range(10):
    word, count = items[i]
    print ("{0:<10}{1:>5}".format(word, count))

```
### Sort函数
#### 列表有自己的sort方法，其对列表进行原址排序
```python
x = [4, 6, 2, 1, 7, 9]
x.sort()
print(x)

```
#### sort和sorted方法还有两个可选参数：key和reverse
#### key接受一个函数，这个函数只接受一个元素，这个函数用于从每个元素中提取一个用于比较的关键字,默认为None
```python
x = ['mmm', 'mm', 'mm', 'm' ]
x.sort(key = len)  #指定key=len，就是比较len()之后的结果
print(x) 

```
#### reverse是一个布尔值。如果设置为True，列表元素将被倒序排列，默认为False
```python
y = [3, 2, 8 ,0 , 1]
y.sort(reverse = False)
print(y) 
```
