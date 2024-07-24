# 本地ARM机子+云服务vps指定端口内穿+开机自启=外网访问本地机子

# 前言

事先声明，本文为个人内网穿透经验，可能不具备复刻性质，自行查验。

## 前期配置

A 腾讯云的VPS一台(配置带宽决定你的上限) --- 我的是Ubuntu，内核amd64

B 你需要内穿的本地arm机子一台 --- 我的是centos，内核arm32

A购买渠道(1核1G最低端的机子也够用了)：

[点我进入优惠渠道](https://github.com/spiritLHL/Hang-up-items#%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%B4%BB%E5%8A%A8)

其他系统具体操作步骤一样的只不过用的命令前缀(下载器)不一样而已。

## 下载两个安装包(一个arm的一个amd64的)

下载地址：[https://github.com/snail007/goproxy/releases](https://github.com/snail007/goproxy/releases)

amd64：![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/gh2.png)

arm：![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/gh1.png)

## 对应每个机子上传对应压缩包

宝塔使用宝塔面板的上传即可，如果没有宝塔，自行使用wget下载到服务器中。

分别上传对应内核的压缩包后，分别解压，如果宝塔内的使用宝塔面板点击解压，如果没有宝塔，自行使用unzip解压。

解压完毕后，只有A也就是你的服务端需要手动安装。

手动安装脚本：

A中执行(对应那个amd64文件解压的路径下)(root权限)

```  
wget https://mirrors.host900.com/https://raw.githubusercontent.com/snail007/goproxy/master/install.sh  
chmod +x install.sh  
./install.sh  
```

## 设置服务器凭证

命令会在本地生成两个文件proxy.crt proxy.key, 相当于账号和密码，用于服务连接的验证。

A中执行(对应那个amd64文件解压的路径下)(root权限)

```
sudo proxy keygen -C proxy
```

## 启动服务端

在云服务器也就是A中启动goproxy的server端

这里设定goproxy服务端与客户端的通信端口为8802。8801属于服务器访问端口，之后我们访问云服务器IP:8801就可以正常访问本地arm机子的5700界面。5700属于本地你需要内穿的端口，如修改这里需要填上自己的。

A中执行(对应那个amd64文件解压的路径下)(root权限)

```
sudo proxy bridge -p ":8802" -C proxy.crt -K proxy.key --daemon
sudo proxy server -r ":8801@:5700" -P "127.0.0.1:8802" -C proxy.crt -K proxy.key --daemon
```

## arm机子部署goproxy客户端

arm机子不需要安装，直接使用解压后的文件。

但要注意，需要上传之前生成的服务器凭证。

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/gh3.png)

上传到与解压文件的同一文件夹下

B中执行(对应那个armV6文件解压的路径下)(root权限)

```
sudo ./proxy client -P 云服务器ip:8802 -C proxy.crt -K proxy.key
```

到此如没有报错访问云服务器IP:8801就可以访问到本地arm机子内网的5700端口了。

但这个启动是需要一直运行的，所以最好安装screen窗口长期挂起，或者直接nohup挂起。

使用ctrl+Z键停止B中的服务。

## A(服务器端)设置开机自启

A机子中，因为执行过一遍就会稳定执行了，不需要自己再挂起任务，所以可以不一定得设置开机自启，你只需要记得每次重启服务器执行一下这行命令即可。

A执行：(这里内容和你上面的命令一样的，参照一下)
```
sudo proxy server -r ":8801@:5700" -P "127.0.0.1:8802" -C proxy.crt -K proxy.key --daemon
```

如果上述命令你记得执行，那就不需要看下面的了，跳转下一步骤。

A中创建一个新文件autostart.sh(随便路径，但最好和你安装goproxy的路径一样，方便记忆)

内容：(这里第二行内容和你上面第一次部署的命令一样的，参照一下)
```
cd /Date-iterms/frp
sudo proxy server -r ":8801@:5700" -P "127.0.0.1:8802" -C proxy.crt -K proxy.key --daemon
```

记住这个文件的路径，保存后打开新路径/etc/init.d

创建新文件ghproxy(注意没有后缀，就是ghproxy)

写入内容：(修改路径为autostart.sh所在路径，这里我的是/Date-iterms/frp，你的不一样自己改)
```bash
#!/bin/sh
### BEGIN INIT INFO
# Provides:          XXX
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: start XXX
# Description:       start XXX
### END INIT INFO

goproxy_path=/Date-iterms/frp #你的那个autostart.sh文件所在路径
case "$1" in
    start)
        echo "start goproxy service.."
        sh ${goproxy_path}/autostart.sh
        ;;
    *)
    exit 1
    ;;
esac
```
保存

A中执行(当前路径下)(root权限)

```
sudo chmod 755 /etc/init.d/ghproxy
cd /etc/init.d
sudo update-rc.d ghproxy defaults
```

若显示未安装update-rc.d，参照

[https://blog.csdn.net/willingtolove/article/details/107494719](https://blog.csdn.net/willingtolove/article/details/107494719)

安装

测试服务是否能启动成功，shell命令如下：
```bash
sudo service ngrok start
```

检查自启动的服务，shell命令如下：
```bash
sudo sysv-rc-conf
```


## B(客户端)设置开机自启

编写脚本，脚本内容如下(创建路径必须为/etc/rc.d/init.d)

这里我的armv6文件解压在/root/proxy中，所以写/root/proxy，不一样的自行更改。
(这里最后一行内容和你上面第一次部署的命令一样的，参照一下)
```
#!/bin/bash
#chkconfig:2345 80 90

cd /root/proxy
sudo ./proxy client -P 服务器IP:8802 -C proxy.crt -K proxy.key
```

B中执行(执行路径/etc/rc.d/init.d)(root权限)

这里第三行有的会执行报错，不用理会
```
chmod +x  /etc/rc.d/init.d/autostart.sh
cd /etc/rc.d/init.d
chkconfig --add autostart.sh
chkconfig autostart.sh on
```

### 此时完成开机自启的所有步骤。

可以重启B试一下是否开机自启了。

# 后言

我的机子是arm32，用的armV6，如果你的机子是别的arm，需要使用的是armV7或armV8。

内穿速度取决于你的服务器速度，带宽啥的。

如果你内穿多个端口，那么只需要在B中链接过A，A中多次执行类似下面这行命令就行了(aaaa是A中端口，bbbb是B中端口)

```
sudo proxy server -r ":aaaa@:bbbb" -P "127.0.0.1:8802" -C proxy.crt -K proxy.key --daemon
```

推一波项目合集：(仓库说明就是合集)

[https://github.com/spiritLHL/Hang-up-items](https://github.com/spiritLHL/Hang-up-items)
