# MySQL数据库04(常见函数)

# 分组函数

功能：作统计使用，又称为聚合函数或统计函数或组函数

---

## 分类：

sum     求和   
avg     平均值   
max     最大值   
min     最小值     
count   计算个数    

---

## 特点：

1.sum，avg一般用于处理数值型，max，min，count可以处理任何类型

2.以上分组函数都忽略```null```值

3.可以和distinct搭配实现去重运算

4、count函数的单独介绍
一般使用count(*)用作统计行数

5、和分组函数一同查询的字段要求是group by后的字段

---

# 1.简单使用

    select SUM(salary) from employees;    
    select AVG(salary) from employees;     
    select MIN(salary) from employees;    
    select MAX(salary) from employees;    
    select count(salary) from employees;     
    (单行执行，一行一行的输出)

    select SUM(salary) 和,AVG(salary) 平均,MAX(salary) 最高,MIN(salary) 最低,count(salary) 个数  from employees;        
    (单行输出全部结果)

---

# 2.参数支持哪些类型

    select SUM(last_name) ,AVG(last_name) from employees;
    (即便不报错，也不支持字符串类型)   

    select SUM(hiredate) ,AVG(hiredate) from employees;   
    (不支持日期类型)   

    select MAX(hiredate) ,MIN(hiredate) from employees;    
    (数值型可用，日期型可用)   

    select MAX(last_name) ,MIN(last_name) from employees;   
    (可排序的都可用)   

    select count(任意类型) from employees;      
    (计算非空值的个数)

---

# 3.和distinc搭配

    select SUM(distinct salary) from employees;     
    (去重后再求和)

    select SUM(*) from employees;   
    (数行数)

    select SUM(1) from employees;   
    (数开头为1的行)

效率：

    MYISAM存储引擎下，count(*)的效率最高    
    INNODB存储引擎下，COUNT (*)和COUNT (1)的效牢差不多，比COUNT(字段)要高一些    

---

# 6.和分组函数一同查询的字段有限制
    
    SELECT AVG (salary) , employee_id FRCM employees;  
    (后面的那个employee_id无法查询但不报错)  

---

