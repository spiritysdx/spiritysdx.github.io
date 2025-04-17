# 给机房的Ubuntu22.04的Linux进行内穿映射端口


## 前言

因为机房的机器没有公网IP，所以需要跳板机进行IP映射内穿，所以需要2样东西：

```
服务器A - 服务端 - 一个有公网IPV4地址的云服务器，IPV4地址本教程假设为 x.x.x.x
```

```
服务器B - 客户端 - 机房内的Ubuntu22.04系统的机器
```

这里选用```goproxy```进行IP内穿

初始环境本教程选择```sudo -i```后的```/root```目录下进行

## 下载goproxy以及预安装proxy

在 [https://github.com/snail007/goproxy/releases](https://github.com/snail007/goproxy/releases) 下载最新的

本教程下载 v14.5 版本，服务器A和服务器B这个东西都得下

(建议下载时进入一个空文件夹中，因为后续这个文件夹会用得到，别直接下载到root目录下)

```
mkdir -p goproxy
cd goproxy
```

```
wget -O proxy-linux-amd64.tar.gz https://cdn.spiritlhl.net/https://github.com/snail007/goproxy/releases/download/v14.5/proxy-linux-amd64.tar.gz && chmod 777 proxy-linux-amd64.tar.gz
```

然后在服务器A中执行

```
wget https://cdn.spiritlhl.net/https://raw.githubusercontent.com/snail007/goproxy/master/install.sh  
chmod +x install.sh  
./install.sh  
rm -rf install.sh
```

执行完毕后执行

```
proxy --version
```

查看是否已安装成功

在服务器A和B中都执行

```
tar -zxvf proxy-linux-amd64.tar.gz
```

解压本体(注意解压到哪个文件夹，下面配置文件中会用到)

## 服务器A中生成服务器凭证

在服务器A中的解压出来的那个文件夹内执行

```
sudo proxy keygen -C proxy
```

命令会在本地生成两个文件```proxy.crt```，```proxy.key```, 相当于账号和密码，用于服务连接的验证。

## 启动和配置服务端的服务器A

在云服务器也就是服务器A中启动goproxy的server端

这里设定goproxy服务端与客户端的通信端口为```8800```，实际可以自行替换自定义

我需要映射服务器B的```22```到```8822```端口上，在A中执行下面的命令启动通信监听

```
sudo proxy bridge -p ":8800" -C proxy.crt -K proxy.key --forever --log proxy.log --daemon
```

执行下述命令启动映射

```
sudo proxy server -r ":8822@:22" -P "127.0.0.1:8800" -C proxy.crt -K proxy.key --forever --log proxy.log --daemon
```



上述命令不是永久生效的，如果你的云服务器重启了，这些命令需要重新执行，所以务必保管好你执行过的命令

## 启动和配置客户端的服务器B

服务器B不需要安装proxy，解压后的文件直接就可以使用了

但需要注意的是，需要上传之前生成的服务器凭证```proxy.crt```和```proxy.key```，上传到与解压后的文件的同一文件夹下，或者cat后自己写同样名字的文件同样的内容到服务器B中


不知道本地路径的执行

```
pwd
```

进行查看，本教程假设为```/root/goproxy```，如果查询不一致，请自己修改后面的文件夹路径

然后需要创建守护进程设置一直运行，需要创建一个systemd的配置文件

```
nano /etc/systemd/system/goproxy_client.service
```

```
[Unit]
Description=goproxy client
After=network.target

[Service]
Type=simple
WorkingDirectory=/root/goproxy
ExecStart=/root/goproxy/proxy client -P x.x.x.x:8800 -C proxy.crt -K proxy.key
User=root
Restart=always
Environment="RUNNER_ALLOW_RUNASROOT=1"

[Install]
WantedBy=multi-user.target
```

保存退出后执行下面的命令启动守护进程

```
systemctl daemon-reload
systemctl start goproxy_client.service
```

查看守护进程

```
systemctl status goproxy_client.service
```

我查询我的服务器B显示

```
root@a12-ThinkStation-P620:~/goproxy# systemctl status goproxy_client.service
● goproxy_client.service - goproxy client
     Loaded: loaded (/etc/systemd/system/goproxy_client.service; disabled; vendor preset: enabled)
     Active: active (running) since Mon 2024-05-13 16:36:46 CST; 8s ago
   Main PID: 12311 (proxy)
      Tasks: 20 (limit: 308965)
     Memory: 26.3M
        CPU: 104ms
     CGroup: /system.slice/goproxy_client.service
             └─12311 /root/goproxy/proxy client -P x.x.x.x:8800 -C proxy.crt -K proxy.key

5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[3] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[4] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[5] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[6] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[7] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[8] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[9] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.967 INFO session worker[10] started
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.968 INFO your system ulimit 524288 is too small, max value is: 524288, try set to 1000000
5月 13 16:36:46 a12-ThinkStation-P620 proxy[12311]: 2024/05/13 16:36:46.968 INFO the result ulimit is: 1000000
```

这就是正常的

你的无问题后执行```ctrl+c```退出查询


开启守护进程开机自启

```
systemctl enable goproxy_client.service
```

如果你需要改动映射的命令，那么修改```/etc/systemd/system/goproxy_client.service```文件后，记得执行

```
systemctl daemon-reload
systemctl restart goproxy_client.service
```

重载配置文件和重启守护进程

## 后言

其他相关说明：https://github.com/snail007/goproxy/blob/master/README_ZH.md
