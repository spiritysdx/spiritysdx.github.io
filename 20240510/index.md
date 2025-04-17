# 在root权限下使用守护进程自托管Github的Runners


## 前置条件

```
cd /root
apt update
apt install curl sudo -y
```

## 安装

执行下述命令

```
export RUNNER_ALLOW_RUNASROOT=1
```

执行后正常安装至于```./config```那一步，然后不要使用官方的启动方式，而是使用守护进程启动，做到runner开机自启以及日志记录

```
nano /etc/systemd/system/github-runner.service
```

添加内容

```
[Unit]
Description=GitHub Actions Runner

[Service]
Type=simple
WorkingDirectory=/root/actions-runner
ExecStart=/root/actions-runner/run.sh
User=root
Restart=always
Environment="RUNNER_ALLOW_RUNASROOT=1"

[Install]
WantedBy=multi-user.target
```

按```ctrl+x```然后按```y```保存然后按两次```enter```退出编辑

然后执行

```
sudo systemctl daemon-reload
sudo systemctl start github-runner
```

启动守护进程

执行

```
sudo systemctl status github-runner
```

查看守护进程状态，如果状态为```active```则说明启动成功，否则就需要排障

查看完毕按```ctrl+c```退出查询，然后执行

```
sudo systemctl enable github-runner
```

设置开机自启

## 后言

这样通过守护进程启动runner可以确保重启服务器后自动启动runner，且日志有记录在守护进程的日志中，也方便查看该进程占用的资源大小

注意，这种方法将在root环境中部署runner，你的action直接在root环境下执行，如果action有问题将可能导致root环境出问题，务必保证使用该runner时action无环境问题。
