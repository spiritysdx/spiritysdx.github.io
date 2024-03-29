# Matlab01(基础命令)

# 基础命令

1.打开文件夹

命令行窗口输入

    cd 文件夹名

这里推荐先在文件管理器先创建后打开

---

2.赋值变量会在工作区显示

可在命令行窗口输入```whos``` 或 ```who```   
可以查看变量属性和具体参数  

---

3.清空数据

    clear

---

4.设置文件搜索路径

![](https://pic./2020/07/01/1b0aba8edf652.png)

---

5.数字类型转换

class()函数可得数字类型

    class(数字)

**整型转换**

int8() #转换成有符号的8位整型

uint8() #转换成无符号的8位整型

**浮点型转换**

数值数据默认为双精度型，可使用

single函数：single(数字)转换成单精度型

double函数：double(数字)转换成双精度型

**复型**

```a+bi```或```a+bj```

real函数：取复型实部数据

image函数：取复型虚部数据

**format命令格式**

    format long %输出长格式

    format %输出短格式

    format rat %输出有理数格式

这个不影响数据存储，只是表达方式不同

ps:```%```是注释符号，按```ctrl+R```注释一行，```ctrl+T```取消一行注释

---

6.常见函数

**三角函数及取整函数**

![](https://pic./2020/07/01/34fa13e83efaa.png)

![](https://pic./2020/07/01/eb8974fcfd129.png)

![](https://pic./2020/07/01/2dd0ac17dfecc.png)

![](https://pic./2020/07/01/8c3309c3dd235.png)

**实战及其他函数**

![](https://pic./2020/07/01/b095473b81ff5.png)

rem(除数,被除数)函数取余数

![](https://pic./2020/07/01/09dfa6f43181f.png)

isprime(n)函数判断n是否为素数，当n是素数时返回1，否则返回0

find()函数找寻数组中的序列号

![](https://pic./2020/07/01/011ba73d7e7ba.png)

---

7.预定义变量

![](https://pic./2020/07/01/eb683aa55d530.png)

![](https://pic./2020/07/01/eb9ef8d19da54.png)

---

# 简单矩阵的建立

1.直接输入法

    A=[1,2,3;4,5,6;7,8,9]

---

2.小矩阵拼接成大矩阵

    A=[1,2,3;4,5,6;7,8,9]    
    B=[-1,-2,3;-4,5,-6;-7,-8,-9]     
    C=[A,B;B,A]  


效果图：

![](https://pic./2020/07/01/0051c0de999f8.png)

---

3.用实部矩阵和虚部矩阵构成复数矩阵

    B=[1,2,3;4,5,6]     
    C=[6,7,8;9,10,11]     
    A=B+i*C  

效果图：

![](https://pic./2020/07/01/91a2326c76d7c.png)

---

# 冒号表达式

![](https://pic./2020/07/01/9c0fd9256862f.png)

---

# 结构矩阵和单元矩阵

![](https://pic./2020/07/01/be15fbe252582.png)

![](https://pic./2020/07/01/51ed61cad6093.png)

---
