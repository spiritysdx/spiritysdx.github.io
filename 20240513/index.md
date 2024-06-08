# 给机房的Ubuntu22.04安装LXD共享GPU资源


## 前言

为什么使用LXD？ 因为宿主机不支持嵌套虚拟化，没有硬件加速没法搞PVE的虚拟机，开容器比较好。由于宿主机是Ubuntu22，自然选择LXD而不是INCUS了，因为后者在ubuntu24以上以及debian13以上才有官方支持包，其他低版本的系统只有第三方编译的包，有问题难处理。

为什么使用官方的WEB端？因为官方支持最完善，虽然其实也就一个壳子，但社区会有管理员管管问题，其他第三方的WEB端更新缓慢且无社区支持。

## 安装LXD

```
snap install lxd -y
apt install zfsutils-linux bridge-utils -y
```

如果要删除 lxd

```
snap remove --purge lxd 
rm -rf /var/lib/lxd 
```

## 初始化LXD

```
/snap/bin/lxd init
```

下面这些选项很重要请手动指定，其他选项未列出的回车使用默认值

```
Name of the storage backend to use (ceph, dir, lvm, zfs, btrfs) [default=zfs]: zfs
Would you like to use an existing empty block device (e.g. a disk or partition)? (yes/no) [default=no]: no
Size in GiB of the new loop device (1GiB minimum) [default=30GiB]: 填你的空闲硬盘大小，我的有1T，所以填1024GiB
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]: no
```

增加第三方镜像源

```
lxc remote add opsmaru https://images.opsmaru.dev/spaces/9bfad87bd318b8f06012059a --public --protocol simplestreams
```

设置CGROUP配置

```
snap set lxd lxcfs.loadavg=true
snap set lxd lxcfs.pidfd=true
snap set lxd lxcfs.cfs=true
systemctl restart snap.lxd.daemon
```

设置镜像不更新

```
lxc config unset images.auto_update_interval
lxc config set images.auto_update_interval 0
```

解除进程数限制

```
if [ -f "/etc/security/limits.conf" ]; then
    if ! grep -q "*          hard    nproc       unlimited" /etc/security/limits.conf; then
        echo '*          hard    nproc       unlimited' | sudo tee -a /etc/security/limits.conf
    fi
    if ! grep -q "*          soft    nproc       unlimited" /etc/security/limits.conf; then
        echo '*          soft    nproc       unlimited' | sudo tee -a /etc/security/limits.conf
    fi
fi
if [ -f "/etc/systemd/logind.conf" ]; then
    if ! grep -q "UserTasksMax=infinity" /etc/systemd/logind.conf; then
        echo 'UserTasksMax=infinity' | sudo tee -a /etc/systemd/logind.conf
    fi
fi
ufw disable
```

然后此时建议重启服务器以解放线程限制

## 启动WEB

```
lxc config set core.https_address 0.0.0.0:8443
snap set lxd ui.enable=true
systemctl reload snap.lxd.daemon
```

由于是机房，需要内穿端口，所以我在跳板机上的服务端(非机房的服务器)执行映射

```
sudo proxy server -r ":8443@:8443" -P "127.0.0.1:8800" -C proxy.crt -K proxy.key --log proxy.log --daemon
```

这条命令看不懂的看我上篇博客

## 配置WEB

打开跳板机的https协议的8443端口，正常进入设置

官方演示视频： https://www.youtube.com/watch?v=wqEH_d8LC1k

通过

```
Generate
Create a new certificate
```

生成证书(生成过程中建议跳过密码验证)，这个证书务必保管好，因为后面要用到。

设置界面有讲解每个主流浏览器的证书设置方法，以下我说的浏览器设置的方法为```EDGE```，其他浏览器类似，可参考上面的官方演示视频。

两个证书文件，一个```crt```一个```pfx```，前者在机房的机器里用，后者在你的浏览器里用。

crt证书文件你可以打开后复制粘贴在机房的机器里创建一个一样内容一样名字的crt文件，这样就不用麻烦的上传crt文件了。crt证书存在后，你在机房的机器中执行下述命令验证

```
lxc config trust add 那个证书名字
```

然后在浏览器中导入pfx证书，注意，其中有一步是问你密码的，留空继续即可，因为前面选生成的时候跳过了，还有一步问你证书保存地址的，选择```根据证书类型，自动选择证书存储(U)```即可。

## 刷新浏览器进入

因为之前进入浏览器是忽略证书校验了，此时刷新也会忽略证书校验，点击不安全那里，然后点击 ```你的证书选择``` 选择 ```LXD UI```开头的名字的证书即可，选择后会自动刷新浏览器进入控制台。

## 开设LXC容器

如下图所示进行开设，注意，Ubuntu 系统最好选择带 LTS 标签的，否则软件源可能出现问题。


![](https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-13_18-40-32.png)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-13_18-40-59.png)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-13_18-41-31.png)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-13_18-51-25.png)

设置完点```create```就开始创建了

## 两种设置显卡的方式

### 使用命令设置

或在机房的机器上使用命令设置(ctnmae我设置为mother，实际请改成自己的容器名字)

```
ctname="mother"
sudo lxc config device add $ctname gpu gpu
sudo lxc config set $ctname security.nesting true
```

不建议启用特权容器

```
# 启用特权容器的命令
ctname="mother"
sudo lxc config set $ctname security.privileged true
```

启用特权容器应该会在```lxc.log```中报错

```
ERROR    conf - ../src/src/lxc/conf.c:turn_into_dependent_mounts:3451 - No such file or directory - Failed to recursively turn old root mount tree into dependent mount. Continuing...
```

如果具有多个显卡，务必指定该容器使用哪张显卡(id后跟显卡序号)

```
ctname="mother"
sudo lxc config device add $ctname gpu-tmp gpu id=0
```

### 使用yaml文件

```
devices: 
  gpu:
    type: gpu
config:
  security.nesting: "true"
  security.privileged: "true"
```

或指定显卡序号(通过id指定序号)

```
devices:
  gpu-tmp:
    id: "0"
    type: gpu
config:
  security.nesting: "true"
  security.privileged: "true"
```

## 容器安装SSH和设置root密码验证登录

进入容器

```
lxc exec mother -- /bin/bash
```

使用我维护的仓库的一键脚本进行配置，使用我的cdn进行加速

仓库：[https://github.com/oneclickvirt/lxd](https://github.com/oneclickvirt/lxd)

下载脚本:

```
apt update && apt install dos2unix -y
curl -o ssh_bash.sh https://cdn.spiritlhl.net/https://raw.githubusercontent.com/oneclickvirt/lxd/main/scripts/ssh_bash.sh && chmod 777 ssh_bash.sh && dos2unix ssh_bash.sh
```

因为是国内服务器，不需要更改DNS，所以需要删除5~10行的内容:

```
sed -i '5,10d' ssh_bash.sh
```

执行脚本：

```
./ssh_bash.sh 你想要设置的密码
```

设置完毕后按```ctrl+a+d```退出容器或执行

```
exit
```

退出容器回到机房的机器(宿主机)

再执行

```
lxc list
```

查询对应容器的IPV4地址替换下面的```10.228.242.7```测试SSH连接

```
ssh root@10.228.242.7
```

然后应该会询问是否继续，输入```yes```后再输入你刚刚设置的容器密码即可，若能正常进入容器，那么证明这一步无问题

## 容器内安装GPU驱动

**容器和宿主机的显卡驱动必须保持一致，否则无法正常使用GPU**，所以必须在容器内外进行驱动查询。

### 容器内外查询显卡驱动

容器外执行

```
nvidia-smi
```

可能的显示为

```
root@a12-ThinkStation-P620:~/lxd# nvidia-smi
Mon May 13 23:42:04 2024       
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.171.04             Driver Version: 535.171.04   CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA RTX A6000               Off | 00000000:61:00.0 Off |                  Off |
| 30%   27C    P8              11W / 300W |    168MiB / 49140MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+
                                                                                         
+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|    0   N/A  N/A      1713      G   /usr/lib/xorg/Xorg                           75MiB |
|    0   N/A  N/A      2108      G   /usr/bin/gnome-shell                         61MiB |
+---------------------------------------------------------------------------------------+
```

可以看到宿主机的驱动版本为```535.171.04```

执行

```
lxc exec mother -- /bin/bash
```

进入mother容器内执行

```
nvidia-smi
```

查询显卡驱动

可能显示如下

```
root@mother:~# nvidia-smi
Command 'nvidia-smi' not found, but can be installed with:
apt install nvidia-utils-510         # version 510.60.02-0ubuntu1, or
apt install nvidia-utils-510-server  # version 510.47.03-0ubuntu3
apt install nvidia-utils-390         # version 390.157-0ubuntu0.22.04.2
apt install nvidia-utils-418-server  # version 418.226.00-0ubuntu5~0.22.04.1
apt install nvidia-utils-450-server  # version 450.248.02-0ubuntu0.22.04.1
apt install nvidia-utils-470         # version 470.223.02-0ubuntu0.22.04.1
apt install nvidia-utils-470-server  # version 470.223.02-0ubuntu0.22.04.1
apt install nvidia-utils-525         # version 525.147.05-0ubuntu0.22.04.1
apt install nvidia-utils-525-server  # version 525.147.05-0ubuntu0.22.04.1
apt install nvidia-utils-535         # version 535.129.03-0ubuntu0.22.04.1
apt install nvidia-utils-535-server  # version 535.129.03-0ubuntu0.22.04.1
```

可以看到容器内的驱动最多支持到```535.129.03```，**容器内外显卡驱动的版本是不同的，不能直接进行安装**.

### 容器内外显卡驱动版本一致时使用官方命令安装

如果你查询得到的版本一致，那么可以使用

```
apt install nvidia-utils-535 -y
```

进行安装，而我的驱动版本不一致，不能这么安装

### 使用Nvidia官方包安装显卡驱动

打开官方网站：[https://www.nvidia.cn/Download/Find.aspx?lang=cn](https://www.nvidia.cn/Download/Find.aspx?lang=cn)

按照你的显卡版本进行选择，我的配置如下，你的与我一般不一致

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240513/circle_screenshot_1_%E5%AE%98%E6%96%B9%E9%AB%98%E7%BA%A7%E9%A9%B1%E5%8A%A8%E6%90%9C%E7%B4%A2%20NVIDIA(1).png)

然后点击```搜索```进行搜索，我搜索结果如下

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240513/(AMD64_EM64T)%20NVIDIA.png)

出现下面这个界面后，别急着点击下载

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240513/circle_screenshot_1_unknown.png)

**右键**点击```同意并开始下载```复制下载链接，在**容器内**使用```wget```下载(**不是宿主机！**)

然后执行(我下到的是```NVIDIA-Linux-x86_64-535.171.04.run```文件)

```
bash NVIDIA-Linux-x86_64-535.171.04.run --no-kernel-module
```

由于容器和宿主机需要共享GPU，所以在安装容器的显卡驱动时需要添加```--no-kernel-module```参数。

安装选项都选择```no```即可，安装好显卡驱动后执行```nvidia-smi```查看显卡驱动是否和宿主机的版本一致。

### 测试是否安装成功

**容器内**执行下述命令安装```Miniconda3```，安装成功后重启容器

```
wget https://repo.anaconda.com/miniconda/Miniconda3-lamother-Linux-x86_64.sh
bash Miniconda3-lamother-Linux-x86_64.sh -b -u
echo 'export PATH="$PATH:$HOME/miniconda3/bin:$HOME/miniconda3/condabin"' >>~/.bashrc
echo 'export PATH="$PATH:$HOME/.local/share/jupyter"' >>~/.bashrc
source ~/.bashrc
sleep 1
echo 'export PATH="/home/user/miniconda3/bin:$PATH"' >>~/.bashrc
source ~/.bashrc
sleep 1
export PATH="/home/user/miniconda3/bin:$PATH"
conda init
```

重启后：

退出base环境

```
conda deactivate
```

启动base环境

```
conda activate base
```

接下来安装```pytorch```，方便测试GPU是否已共享成功

打开

[https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/)

或

[https://pytorch.org/get-started/previous-versions/](https://pytorch.org/get-started/previous-versions/)

查看对应版本安装，由于我的cuda版本是```12.2```，所以我执行了以下命令，其他版本自行查询
(下面pytorch-cuda用的12.1这个版本是因为没有12.2的版本，所以只能退而求其次使用低一个版本的)

```
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

安装过程中一直按回车即可

测试是否可以调用显卡，下面的命令执行显示```true```则没问题

```
(base) root@mother:~# python                                                                                                                                                
Python 3.12.2 | packaged by Anaconda, Inc. | (main, Feb 27 2024, 17:35:02) [GCC 11.2.0] on linux                                                                       
Type "help", "copyright", "credits" or "license" for more information.                                                                                                      
>>> import torch                          
>>> print(torch.cuda.is_available())    
True                                   
>>>
```

测试没问题，按```ctrl+z```退出conda的python窗口

## 共享容器文件夹和宿主机文件夹

为方便文件的传输与节约内存使用，需要设置共享文件夹来实现宿主机与容器之间的文件传输。

在宿主机的```/root```路径下执行下述命令创建文件夹和设置权限

```
mkdir sharefile
chmod 777 sharefile
```

在宿主机创建共享区域share-host

```
lxc profile create share-host
lxc profile set share-host security.privileged true
```

检查是否创建成功

```
lxc profile show share-host
```

将文件夹添加到制定LXD容器mother中

```
ctname="mother"
lxc config device add $ctname share-host disk source=/root/sharefile path=/root/sharefile
```

## 多用户创建

模板容器```mother```已经配置好了，执行```lxc stop mother```以停止容器方便复制

克隆容器 参数一为模板容器名称，参数二为目标容器名称

```
lxc copy mother user1
```

启动用户1的容器

```
lxc start user1
```

一定要保证模板容器在copy前是stopped状态，否则你复制会复制带上内网IP地址，复制会导致冲突。

## 后言

至于多用户下的容器，对应容器需要映射不同的端口，后续专门写一篇讲讲怎么配置
