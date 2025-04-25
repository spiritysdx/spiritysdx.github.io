# go-cqhttp搭建教程


## 官方安装教程

[官方教程](https://docs.go-cqhttp.org/guide/quick_start.html)

## 个人使用经验

压缩包里的是适配Linux环境的二进制文件

按照官方文档操作后，发现一些小问题

首先是```config.yml```的部分配置应该是这样子的

(我只使用http协议)

```
  uin:  # 必填QQ账号
  password: '' # 千万别填
```

只做推送用不需要数据库监控
```
database: # 数据库相关设置
  leveldb:
    # 是否启用内置leveldb数据库
    # 启用将会增加10-20MB的内存占用和一定的磁盘空间
    # 关闭将无法使用 撤回 回复 get_msg 等上下文相关功能
    enable: false
```

服务器这块需要改```0.0.0.0```

```
  # HTTP 通信设置
  - http:
      # 服务端得填0.0.0.0
      host: 0.0.0.0
      # 服务端监听端口，只要是端口开放了没被占用就行
      port: 3500
```

下面是我使用的版本的压缩包，上传宝塔中的一个空文件夹里解压参照官方文档和我的个人经验使用即可

**本人使用的go-cqhttp压缩包：[博客相关资源-go-cqhttp](https://www.spiritlhl.top/ziyuan/)**

如果是参照官方文档搭的本地版本

本地搭建这块需要改```127.0.0.1```

```
  # HTTP 通信设置
  - http:
      # 本地搭建得填127.0.0.1
      host: 127.0.0.1
      # 服务端监听端口，只要是端口开放了没被占用就行
      port: 3500
```

云服务器需要使用```screen```命令后台24小时运行

在go-cqhttp文件所在的文件夹终端中输入

```
screen -S QQ
```

创建会话窗口

然后输入

```
sudo ./go-cqhttp
```

运行程序

等待初始化后，QQ扫码登陆即可

登陆完成后

在当前会话窗口中按住```Ctrl```,```a```,```d```三个快捷键可以实现分离会话窗口，这时窗口会弹出[detached]的提示，并回到主窗口，此时可以关闭终端，已经成功挂上了。

如果想要恢复查看

终端输入

```
screen -ls
```
显示

```
There is a screen on:

2637.QQ (12/17/2015/10:00:32 AM) (Detached)
```

终端输入

```
screen -r 2637 
```
进入2637线程，恢复QQ会话窗口

或

```
screen -r QQ
```

这样就能回到QQ窗口了

如果输入```screen -ls```后看到QQ窗口后面的(???dead)字样，说明窗口死了，但是仍在占用空间。这时需要清除窗口

输入
```
screen -wipe 
```
自动清除死去的窗口

ps:突然断网导致无法重登窗口

首先使用screen -d *****(id)，先退出，然后再使用 screen -r *****(id)重新连接

鸣谢：

[参考的csdn博客](https://blog.csdn.net/hejunqing14/article/details/50338161)

# 欢迎请站长喝一杯

![](https://i.loli.net/2021/07/15/UPk5VbzAIC6OM7y.jpg)
