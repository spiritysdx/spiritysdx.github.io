# MySQL数据库01(cmder基本运行指令)

# 前言

MySQL数据库下载链接(5.5版本)

[https://spiritlhl.lanzous.com/id5vt2f](https://spiritlhl.lanzous.com/id5vt2f)

# 常用指令

1.查看当前所有的数据库  
```show dat abases;```
2.打开指定的库 
```use 库名;```
3.查看当前库的所有表
```show_ tables;```
4.查看其它库的所有表   
```show tables from 库名;```   
5.创建表   
```
create table 表名(   
列名 列类型，
列名 列类型，
...
)
```
6.查看表结构   
```desc表名;``` 

---

# 启动与退出

```
#cmder 管理员模式下

C:\Users\祈LHL\Desktop                                                                   
$ net stop mysql                                                                        
MySQL 服务正在停止.                                                                           
MySQL 服务已成功停止。                                                                          
                                                                                        
                                                                                        
C:\Users\祈LHL\Desktop                                                                   
$ net start mysql                                                                       
MySQL 服务正在启动 .                                                                          
MySQL 服务已经启动成功。                                                                         
                                                                                        
```

或者直接打开控制面板，搜索```服务```在服务中找到MySQL右键```属性```选择```手动```或```开机自启```，右键选择```打开```或```关闭```服务

---

# 登录与退出登录

```
#cmder 管理员模式下

C:\Users\祈LHL\Desktop                                                                   
$ mysql -h localhost -P3306 -u root -p                                             

Enter password: 你的root账号密码                                             

#这里的【-h主机名 -P端口号】在本地客户端里可以不输入， -u用户名 -p密码

Welcome to the MySQL monitor.  Commands end with ; or \g.                               
Your MySQL connection id is 2                                                           
Server version: 5.5.62 MySQL Community Server (GPL)                                     
                                                                                        
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.            
                                                                                        
Oracle is a registered trademark of Oracle Corporation and/or its                       
affiliates. Other names may be trademarks of their respective                           
owners.                                                                                 
                                                                                        
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.          
                                                                                        
mysql> exit                                                                             
Bye                                                                                     
```     

或直接在左下角```开始```图标里找到```MySQL 5.5 Command Line Client```点击打开输入root密码，退出输入```exit```或```ctrl+c```

---

# 显示存在的目录

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.01 sec)

mysql> use test;#进入test库
Database changed
mysql> show tables;#展示表
Empty set (0.01 sec)

mysql> show tables from mysql;
#这里还在test库，只是去查看mysql库，没有离开test库
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| db                        |
| event                     |
| func                      |
| general_log               |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| host                      |
| ndb_binlog_index          |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| servers                   |
| slow_log                  |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
24 rows in set (0.00 sec)

mysql> select database();
#查看在哪个库
+------------+
| database() |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)
```

---

# 创建表

```
mysql>  create table stuinfo(
    -> id int,
    -> name varchar(20));
Query OK, 0 rows affected (0.02 sec)
```

---

# 查看表

```
# desc 加 空格 加 表名
mysql> desc stuinfo;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.02 sec)
```

---

# 查看表内数据

```
select * from stuinfo;
```

---

# 小技巧

如果你之前输入过相同命令且与你的输入行接近，按键盘上下键可以切换你输入过的命令，可以避免重复输入相同命令消耗时间，cmder下可以选中命令后右键点击，自动复制该命令到输入的行

---

# 查看数据库版本

```
#已经登陆时输入(SQL指令)
mysql> select version();
+-----------+
| version() |
+-----------+
| 5.5.62    |
+-----------+
1 row in set (0.01 sec)
```

```
#未登录时输入(windows指令)
$ mysql --version
mysql  Ver 14.14 Distrib 5.5.62, for Win64 (AMD64)

#或者输入(windows指令)

mysql -V
mysql  Ver 14.14 Distrib 5.5.62, for Win64 (AMD64)
```

---

