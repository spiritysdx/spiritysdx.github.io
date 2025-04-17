# MySQL数据库03(常见函数)


# 函数

概念:类似于java的方法，将-组逻辑语句封装在方法体中，对外暴露方法名

好处: 

    1、隐藏了实现细节     
    2、 提高代码的重用性    

调用:

 select 函数名(实参列表) [ from 表] ;  

特点:

    1.叫什么(函数名)    
    2.干什么(函数功能)    

分类:

1、单行函数

如concat、 length、 ifnull等

2、分组函数

功能:做统计使用，又称为统计函数、聚合函数、组函数

### length获取参数值的**字节**个数

    SELECT LENGTH('john') ;   
    SELECT LENGTH(' 张三丰hahaha') ;   

    SHOW VARIABLES LIKE '%char%'

---

### concat拼接字符串

    SELECT CONCAT (last_ name, '_ ' , first_ name) 姓名 FROM employees;

---

### upper、lower

    SELECT UPPER('john') ;   
    SELECT LOWER( 'joHn') ;    
 
示例:将姓变大写，名变小写，然后拼接

    SELECT CONCAT (UPPER(last_ name) , LOWER(first_ name) )姓名 FROM employees;   

---

### substr、substring

注意:索引从1开始

    #截取从指定索引处后面所有字符     
    SELECT SUBSTR('欢迎来到二叉树的博客',7)out_put;  

    #截取从指定索引处指定字符长度的字符   
    SELECT SUBSTR('欢迎来到二叉树的博客',5,7) out_put;   

案例:姓名中首字符大写，其他字符小写然后用_拼接，显示出来    

    SELECT CONCAT (UPPER (SUBSTR(last_ name,1,1)) ,'_ ' , LOWER (SUBSTR(last_ name,2)) ) out_put   
    FROM employees ;

---

### instr

返回子串第一次出现的索引，如果找不到返回0

    SELECT INSTR('欢迎来到二叉树的博客', '二叉树') AS out_ put;

---

### trim
    SELECT LENGTH (TRIM('二叉树')) AS out_put;
    SELECT TRIM('aa' FROM 'aaaaaaaa二aaaaaaaaaaa叉树aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa ')
    AS out_put;

---

### lpad

用指定的字符实现左填充指定长度

    SELECT LPAD('二叉树',2, 1*1) AS out_ put;

---

### rpad

用指定的字符实现右填充指定长度

    SELECT RPAD('二叉树',12, 'ab') AS out_ put;

---

### replace

替换

    SELECT REPLACE('欢迎你来到二叉树的博客', '你'，'大家') AS out_ put;

---

# 数学函数

### round

四舍五入

    SELECT ROUND(-1.55) ;    
    SELECT ROUND(1.567,2) ;   

---

### ceil

向上取整,返回>=该参数的最小整数

    SELECT CEIL(-1.02) ;

---

### floor

向下取整，返回<=该参数的最大整数

    SELECT FLOOR(-9.99) ;

---

### truncate

截断

    SELECT TRUNCATE (1.69999,1) ;

---

### mod

取余

/*

    mod(a,b) ;   a-a/b*b

    mod(-10,-3) :-10- (-10)/ (-3)* (-3) =-1
*/

    SELECT MOD(10,-3) ;    
    SELECT 10号3;    
 
---

# 日期函数

### now

返回当前系统日期+时间

    SELECT NOW() ;    

### curdate

返回当前系统日期，不包含时间

    SELECT CURDATE() ;

---

### curtime

返回当前时间，不包含日期

    SELECT CURTIME () ;

---

### 可以获取指定的部分年、月、日、小时、分钟、秒

    SELECT YEAR (NOW())年;    
    SELECT YEAR('1998-1-1') 年;   
    SELECT YEAR (hiredate)年FROM employees;    
    SELECT MONTH (NOW())月;     
    SELECT MONTHNAME (NOW())月;     

---

### 补充

now:获取当前日期

    str_to_date: 将日期格式的字符转换成指定格式的日期

    示例：STR_TO_DATE('9-13-1999','%m-%d-%Y')

    结果：1999-09-13

date_ format:将日期转换成字符

    DATE_ FORMAT('2018/6/6','%Y年%m月 %d日)

    结果：2018年06月06日

对照图：

(https://pic./2020/06/02/71c8a64dc5fee.png)

---

# 流程控制函数

### if函数: if else的效果

    SELECT IF(10<5, '大', '小');     
    SELECT last_ name, commission_ _pct, IF (commission_ pct Is NULL, '没奖金', '有奖金')备注    
    FROM employees;     

### case

    when条件1 then 要显示的值1或语句1      
    when条件2 then 要显示的值2或语句2      
    else要显示的值n或语句n      
    end     

案例:查询员工的工资的情况

如果工资>20000,显示A级别     
如果工资>15000,显示B级别     
如果工资>10000，显示c级别     
 
    SELECT salary,    
    CASE     
    WHEN salary>20000 THEN 'A'   
    WHEN salary>15000 THEN 'B'   
    WHEN salary>10000 THEN 'C'  
    ELSE 'D'    
    END AS 工资级别   
    FROM employees ;    

