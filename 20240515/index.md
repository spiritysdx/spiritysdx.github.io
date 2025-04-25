# 给机房的LXD容器配置跳板机进行连接


## 前言

由于机房的服务器(下称宿主机)都是没有公网IP的(只有NAT的IPV4网络)，所以需要通过跳板机进行连接。

由于Pycharm的远程连接是通过SSH进行连接的，使用SFTP协议，所以仅使用22端口即可。

## 解除跳板机进程限制

由于到时候需要在跳板机上搭建很多映射，需要提前解放进程限制，在跳板机上执行以下命令

```
# 更新 /etc/security/limits.conf 文件
echo "更新 /etc/security/limits.conf 文件..."
sed -i '/^root soft nofile /d' /etc/security/limits.conf
sed -i '/^root hard nofile /d' /etc/security/limits.conf
sed -i '/^\* soft nofile /d' /etc/security/limits.conf
sed -i '/^\* hard nofile /d' /etc/security/limits.conf
echo "root soft nofile 1000000" >> /etc/security/limits.conf
echo "root hard nofile 1000000" >> /etc/security/limits.conf
echo "* soft nofile 1000000" >> /etc/security/limits.conf
echo "* hard nofile 1000000" >> /etc/security/limits.conf

# 更新 /etc/pam.d/common-session 文件
echo "更新 /etc/pam.d/common-session 文件..."
if ! grep -q "session required pam_limits.so" /etc/pam.d/common-session; then
  echo "session required pam_limits.so" >> /etc/pam.d/common-session
fi

# 更新 /etc/pam.d/common-session-noninteractive 文件
echo "更新 /etc/pam.d/common-session-noninteractive 文件..."
if ! grep -q "session required pam_limits.so" /etc/pam.d/common-session-noninteractive; then
  echo "session required pam_limits.so" >> /etc/pam.d/common-session-noninteractive
fi

# 更新 /etc/systemd/system.conf 文件
echo "更新 /etc/systemd/system.conf 文件..."
sed -i '/^DefaultLimitNOFILE=/d' /etc/systemd/system.conf
echo "DefaultLimitNOFILE=1000000" >> /etc/systemd/system.conf

# 更新 /etc/systemd/user.conf 文件
echo "更新 /etc/systemd/user.conf 文件..."
sed -i '/^DefaultLimitNOFILE=/d' /etc/systemd/user.conf
echo "DefaultLimitNOFILE=1000000" >> /etc/systemd/user.conf

sleep 1

systemctl daemon-reload
```

然重启跳板机，再执行

```
ulimit -n
```

查询当前进程限制，应当显示```1000000```

## 端口转发配置

首先需要将容器的内网22端口映射到宿主机的公共端口上，我选择将user1容器的22端口映射到宿主机的10022端口上。

宿主机执行：

```
ctname="user1"
ctssh="10022"
lxc config device add "$ctname" ssh-port proxy listen=tcp:0.0.0.0:$ctssh connect=tcp:127.0.0.1:22
```

然后需要将宿主机的10022端口映射出来，映射到跳板机上的10022端口上。

跳板机执行：

```
tb_port="10022" # 跳板机上的端口
sz_port="10022" # 宿主机上的端口
sudo proxy server -r ":${tb_port}@:${sz_port}" -P "127.0.0.1:8800" -C proxy.crt -K proxy.key --log proxy.log --daemon
```

其他端口也按照对应的方式映射，自行修改端口号即可。

## 后言

上述在跳板机上的所有配置均为一次性配置，务必在重启跳板机后，重新执行这些映射命令，不会自动保留映射关系。

重启跳板机后务必事先设置本地监听端口8800，我的配置如下

```
sudo proxy bridge -p ":8800" -C proxy.crt -K proxy.key --forever --log proxy.log --daemon
```

然后再进行端口映射等操作。
