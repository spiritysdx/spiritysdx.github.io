# CentOS8的VNC窗口桌面使用


## CentOS8安装VNC窗口桌面并使用

### Step1 安装vnc软件
终端窗口输入

```bash
sudo -i
dnf install tigervnc tigervnc-server
```
### Step2 修改vncserver-config-defaults ， 如果添加一行localhost ，外部不能访问

终端窗口输入

```bash
vim /etc/tigervnc/vncserver-config-defaults
```

按```i```键进入编辑模式，复制粘贴下面内容替换原来的内容

```bash
session=gnome
securitytypes=vncauth,tlsvnc
desktop=sandbox
geometry=2000x1200
alwaysshared
```

按```Esc```键关闭编辑模式，输入

```bash
:wq
```

再按回车退出vim，下面编辑方式一样，不再赘述，将内容和打开文件命令变一下即可。

### Step3 编辑vncserver.users

终端窗口输入

```bash
vim /etc/tigervnc/vncserver.users
```

替换内容

```bash
:1=root
:2=admin
```

### Step4 配置vnc密码

终端

```
[root@OS-CentOS8 ~]$vncpasswd
Password:你的登陆密码，输入不会显示，自己记住
Verify:你的登陆密码，输入不会显示，自己记住
Would you like to enter a view-only password (y/n)? n
A view-only password is not used
```

### Step5 复制vncserver@x.service 文件

终端输入

```bash
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
```

### Step6 配置vnc开机自启服务

终端输入

```bash
systemctl enable vncserver@:1
```

### Step7 启动vnc服务

终端输入

```bash
systemctl start vncserver@:1
```

### Step8 查看状态

终端输入

```bash
systemctl status vncserver@\:1
```

看到绿色和active字样就是成功安装并启动了。

### Step9 vncview客户端连接

文件下载链接：

[太平洋下载](https://dl.pconline.com.cn/download/2295935.html)

使用说明：

[简书说明，从Step2开始看，前面安装不用看，点我跳转](https://www.jianshu.com/p/105882b83706)

### 后言

除了不能使用exe文件，其他和win系统没啥区别，浏览器默认安装火狐的好像，不用更新，不然占用会很大。

# 欢迎请站长喝一杯

![](https://i.loli.net/2021/07/15/UPk5VbzAIC6OM7y.jpg)
