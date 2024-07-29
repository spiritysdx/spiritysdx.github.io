# MySQL数据库02(图形界面客户端基本运行指令)

# 前言

图形界面客户端下载链接

[https://spiritlhl.lanzous.com/id5vuji](https://spiritlhl.lanzous.com/id5vuji)

# 客户端安装

1.配置证书    
2.新建用户配置   
3.启用   
4.询问(navaicate)窗口是命令行窗口   
5.ctrl+s保存指令   
6.ctrl+鼠标滚动轴调整字体大小   
7.指令末尾加分号   

---

# DQL语言(Data Query Language)

### 基础查询

    ues 上级表名;  
    (打开你要查询的上级表/库名)     
    (这里点击查询的库也能进入，但不推荐使用这种方法，尽量用SQL命令执行)    
    select 查询列表 from 表名;  
    (查询列表可以是表中字段，常量值，表达式，函数)  
    (查询结果是一个虚拟表格)  

---

**查询表中字段**

1.查询单个字段

    select xxx from 上级表名;

2.查询多个字段

    select 字段1,字段2,字段3 from 表名;   
    (这里的顺序与表内顺序无关，只是显示顺序)  

3.查询表中所有字段

    select (双击表名，此处会显示`所选表名`) from 上级表名;    
    (这里的表名顺序无要求，是展示的顺序)    
    (格式切换是选中所有指令后按Fn+F12，格式化)    
    select * from 上级表名;    
    (这种方式选中的表顺序与源顺序一样)    

(这里的`是着重号不是单引号，着重号主要用于区分表名为保留字，其他时候可有可无)       

(执行命令选中指令按Fn+F9或者按栏目里的执行按钮)   

---

**查询常量值**

    select 数字常量;   
    select 字段常量;

---

**查询表达式**

    select 100*333;   
    (数字运算)

---

**查询函数**

    select version();

---

### 为字段起别名

**方式一**

使用as

    select 100*333 as 结果;

    select last_name as 姓,first_name as 名 from 上级表名;

    select   xxx as yyy, kkk from 上级表名

好处：

    1.便于理解       
    2.如果要查询的字段有重名，使用别名区分开来   
    3.别名在表头显示

**方式二**

使用空格

    select last_name 姓,first_name 名 from 上级表名;

这里建议别名与保留字重复时用双引号或单引号括起来

---

### 去重

    select distinct 表中字段 from 上级表名;

---

### +号作用

仅仅表示运算符

    select 100+90;                   两个操作数都为数值型，则做加法运算      
    select '123'+90;                 其中一方为字符型，试图将字符型数值转换成数值型       
    select 'john'+90;                如果转换成功，则继续做加法运算，如果转换失败，则将字符型数值转换成0    
    select nu11+10;                  只要其中一方为nu11，则结果肯定为nu11    

---

### 拼接

调用concat函数

    select concat('a','b','c')

---

### 判断是否为空

    select ifnull(表名,返回值)

---

### 条件查询

语法

    select
           查询列表
    from
            表名
    where
            筛选条件

分类

    1.按条件表达式筛选      
        条件运算符:   > < = <= >= != <>   
        (最后一个是不等号)   
    2.按逻辑表达式筛选   
        逻辑运算符：   
            &与 ||或 !非   
            and  or  not   
    3.模糊查询
        like   
        between and   
        in   
        is null   

---

**按条件表达式筛选**

案例1:查询收入大于10000的人员信息

指令如下

    select
            *
    from
            employees
    where
            salary>10000;

案例2:查询部门编号不等于90的员工名称

指令如下

    select
            last_name,
            department_id
    from
            employees
    where
            department_id != 90;

---

**按逻辑表达式筛选**

&&和and:两个条件都为true,结果为true，反之为false    
||或or: 只要有一个条件为true,结果为true, 反之为false   
|或not:_如果连按的条件本身为false, 结果为true,反之为falsd    

案例3：查询工资z在10000到20000之.间的员工名、工资以及奖金

    SELECT
            last_ name ,
            salary,
            commission_ pct
    FROM
            employees
    WHERE
            salary>=10000 AND salary<=2 0000;

案例4:查询部门编号不是在90到110之间，或者工资高于15000的员工信息

    SELECT
            *
    FROM
            employees
    WHERE
            NOT (department_ id>=90 AND department_ id<=110) OR salary>15000;


---

### 模糊查询

**like**

特点:  

    一般和通配符搭配使用   
    通配符:   
    %   任意多个字符,包含0个字符   
    _   任意单个字符   

案例5:查询员工名中包含字符a的员工信息

    SELECT
            *
    FROM
            employees
    WHERE
            last_ name LIKE '%a%' ;#abc

案例6:查询员工名中第三个字符为e，第五个字符为a的员工名和工资

    SELECT
            last_ name,
            salary
    FROM
            employees
    WHERE
            last_ name LIKE '__n_l%';|


案例7:查询员工名中第二个字符为_的员工名

    SELECT
            last_ name
    FROM
            employees
    WHERE
            last_ name LIKE '_ $_%' ESCAPE '$' ;

或者

    '_\_%'也能转义_

---

**between and**


 I
案例3:查询员工编号在100到120之间的员工信息

    SELECT
            *
    FROM
            employees
    WHERE
            employee_ _id >= 100 AND employee_ id<=120;

#----------------------等同于

    SELECT
            *
    FROM
            employees
    WHERE
            employee_ id BETWEEN 100 AND 120; .

这里的

    employee_ id BETWEEN 大于等于的值 AND 小于等于的值;
 
---

**in**

含义:判断某字段的值是否属于in列表中的某一项

特点:

    1.使用in提高语句简洁度
    2.in列表的值类型必须一致或兼容
    3.不能和通配符混用


案例8:查询员工的工种编号是IT_PROG、 AD_VP、 AD_PRES中的一个员工名和工种编号

    SELECT
            last_ name,
            job_ id
    FROM
            employees
    WHERE
            job_ id = 'IT_ PROT' OR job_ id = 'AD_ VP' OR JOB_ ID ='AD_ PRES' ;


这里的

    job_id = 'IT_ PROT' OR job_id = 'AD_ VP' OR JOB_ID ='AD_ PRES' ;

等同于

    job_id IN('T_PROT','AD_ VP', 'AD_ PRES');

---

**is null**

=或<>不能用于判断null值

is null或is not null 可以判断null值


案例9：查询没有奖金的员工名和奖金率

    SELECT
            last_ name，.
            commission_pct
    FROM
            employees
    WHERE
            commission_pct IS NULL;

如果是查询有奖金的，写```is not null```

---

**安全等于 <==>**

<==>可以判断null值也可以判断一般数值

Is NULL: 仅仅可以判断NULL值，可读性较高，建议使用

<=>: 既可以判断NULL值，又可以判断普通的数值，可读性较低

---

# 排序查询

引入:

    select * from employees ;

语法:

    select  查询列表
    from    表
    [where筛选条件]
    order by 排序列表 [asc|desc]

案例:查询员工信息，要求工资从高到低排序

    SELECT * FROM employees ORDER BY salary DESC;

案例:查询员工信息，要求工资从低到高排序

   SELECT * FROM employees ORDER BY salary ASC; 

特点:

1.asc代表的是升序，desc代表的是降序   

2.如果不写，默认是升序    

3.支持按表达式，别名，函数排序

4.order by子句一般是放在查询语句的最后面，limit子句除外


案例:按姓名的长度显示员工的姓名和工资[按函数排序]

    SELECT LENGTH(last_ name) 字节长度,last_ name, salary
    FROM employees
    ORDER BY LENGTH (last_ name) DESC;

案例:查询员工信息，要求先按工资升序，再按员工编号降序[按多个字段排序]
    SELECT *
    FROM employees
    ORDER BY salary ASC, employee_ _id DESC;


