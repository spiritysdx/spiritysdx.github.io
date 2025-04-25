# 使用LXD对服务器进行LXC容器切分


### 使用LXD对服务器进行LXC容器切分(下面简称母鸡开小鸡)

Linux母鸡开小鸡，一键多开NAT小鸡，一键LXC虚拟化，一键多开服务器，多开容器，一键多开NAT小鸡，一键多开NAT服务器

上述需求都得到了解决

### 自写仓库链接

[https://github.com/spiritLHLS/lxd](https://github.com/spiritLHLS/lxd)

### 实验性一键脚本

环境要求：必须为Ubuntu系统

一键安装lxd环境

```bash
curl -L https://raw.githubusercontent.com/spiritLHLS/lxd/main/lxdinstall.sh -o lxdinstall.sh && chmod +x lxdinstall.sh
```

设置母鸡内存虚拟化大小以及资源池硬盘大小

```bash
./lxdinstall.sh 内存大小以MB计算 硬盘大小以GB计算
```

一键安装开lxd母鸡所需要的带vnstat环境的常用预配置环境

(实际主体功能是一键安装vnstat工具)

```bash
curl -L https://raw.githubusercontent.com/spiritLHLS/lxd/main/backend.sh -o backend.sh && chmod +x backend.sh && bash backend.sh
```

### 如果上述一键安装出现问题，建议使用手动+自动结合的方式，如下

同时进行TCP和UDP转发，除了SSH端口其他的映射内网外网端口一致，且只适用于Ubuntu或Debian，推荐Ubuntu20或Ubuntu更低版本，debian系列多半有问题

一定要在 /root 的路径下运行本脚本！

#### 普通版本(带1个SSH端口，25个外网端口)

开出的小鸡配置：1核256MB内存1GB硬盘限速250MB

自动关闭防火墙

```bash
apt update
apt install curl wget sudo dos2unix ufw -y
ufw disable
```

内存看你开多少小鸡，这里如果要开8个，换算需要2G内存，实际内存如果是512MB内存，还需要开1.5G，保守点开2G虚拟内存即可

执行下面命令，输入1，再输入2048，代表开2G虚拟内存

```
curl -L https://raw.githubusercontent.com/spiritLHLS/lxd/main/swap.sh -o swap.sh && chmod +x swap.sh && bash swap.sh
```

实际swap开的虚拟内存应该是实际内存的2倍，也就是开1G是合理的，上面我描述的情况属于超开了

```
apt install snapd -y
snap install lxd
/snap/bin/lxd init
```

如果上面的命令中出现下面的错误

(snap "lxd" assumes unsupported features: snapd2.39 (try to update snapd and refresh the core snap))

使用命令修补后再进行lxd的安装

```
snap install core
```

如果无异常，上面三行命令执行结果如下

![](https://i.bmp.ovh/imgs/2022/06/01/76dd73f43e138c88.png)

一般的选项回车默认即可

选择配置物理盘大小(提示默认最小1GB那个选项)，一般我填空闲磁盘大小减去内存大小后乘以0.95并向下取整

提示带auto的更新image的选项记得选no，避免更新占用

软连接lxc命令

```bash
! lxc -h >/dev/null 2>&1 && echo 'alias lxc="/snap/bin/lxc"' >> /root/.bashrc && source /root/.bashrc
export PATH=$PATH:/snap/bin
```

测试lxc有没有软连接上

```
lxc -h
```

lxc命令无问题，执行初始化开小鸡，这一步最好放screen中后台挂起执行，开小鸡时长与你开几个和母鸡配置相关

执行下面命令加载开机脚本

```
# 初始化
rm -rf init.sh
wget https://github.com/spiritLHLS/lxd/raw/main/init.sh
chmod 777 init.sh
apt install dos2unix -y
dos2unix init.sh
```

下面命令为开小鸡名字前缀为**tj**的**10**个小鸡

```
./init.sh tj 10
```

有时候init.sh的运行路径有问题，此时建议前面加上sudo强制根目录执行



#### 纯探针版本(只有一个SSH端口)

开出的小鸡配置：1核128MB内存300MB硬盘限速200MB

自动关闭防火墙

```bash
apt update
apt install curl wget sudo dos2unix ufw -y
ufw disable
```

内存看你开多少小鸡，这里如果要开10个，换算需要1G内存，实际内存如果是512MB内存，还需要开0.5G，保守点开1G虚拟内存即可

执行下面命令，输入1，再输入1024，代表开1G虚拟内存

```bash
curl -L https://raw.githubusercontent.com/spiritLHLS/lxd/main/swap.sh -o swap.sh && chmod +x swap.sh && bash swap.sh
```

实际swap开的虚拟内存应该是实际内存的2倍，也就是开1G是合理的，再多就超开了

```
apt install snapd -y
snap install lxd
/snap/bin/lxd init
```

如果上面的命令中出现下面的错误

(snap "lxd" assumes unsupported features: snapd2.39 (try to update snapd and refresh the core snap))

使用命令修补后再进行lxd的安装

```
snap install core
```

如果无异常，上面三行命令执行结果如下

![](https://i.bmp.ovh/imgs/2022/06/01/76dd73f43e138c88.png)

一般的选项回车默认即可

选择配置物理盘大小(提示默认最小1GB那行)，一般我填空闲磁盘大小减去内存大小后乘以0.95并向下取整

提示带auto的更新image的选项记得选no，避免更新占用

软连接lxc命令

```bash
! lxc -h >/dev/null 2>&1 && echo 'alias lxc="/snap/bin/lxc"' >> /root/.bashrc && source /root/.bashrc
export PATH=$PATH:/snap/bin
```

测试lxc有没有软连接上

```
lxc -h
```

lxc命令无问题，执行初始化开小鸡，这一步最好放screen中后台挂起执行，开小鸡时长与你开几个和母鸡配置相关

加载开机脚本

```
# 初始化
rm -rf least.sh
wget https://github.com/spiritLHLS/lxd/raw/main/least.sh
chmod 777 least.sh
apt install dos2unix -y
dos2unix least.sh
```

下列命令最后一行为开小鸡名字前缀为**tj**的**10**个小鸡

```
./least.sh tj 10
```

有时候least.sh的运行路径有问题，此时建议前面加上sudo强制根目录执行

### 开完多个容器后，具体信息会生成在当前目录下的log文件中，格式如下

```
1号服务器名称 密码 ssh端口 外网端口起始 外网端口终止
2号服务器名称 密码 ssh端口 外网端口起始 外网端口终止
```

### 只开一个容器

预装环境如上面的那些一样

加载开机脚本

```
# 初始化
rm -rf buildone.sh
wget https://github.com/spiritLHLS/lxd/raw/main/buildone.sh
chmod 777 buildone.sh
apt install dos2unix -y
dos2unix buildone.sh
```

开鸡

内存大小以MB计算，硬盘大小以GB计算

```
./buildone.sh 小鸡名称 内存大小 硬盘大小 SSH端口 外网起端口 外网止端口
```

### 常用LXC命令


查看所有

```bash
lxc list
```

查看个例

```bash
lxc info 服务器名字
```

启动个例

```bash
lxc start 服务器名字
```

停止个例

```bash
lxc stop 服务器名字
```

删除个例

```bash
lxc rm -f 服务器名字
```

## 更多更新以及使用方式以仓库说明为准

## 备份仓库：

[https://gitlab.com/spiritysdx/lxc](https://gitlab.com/spiritysdx/lxc)
