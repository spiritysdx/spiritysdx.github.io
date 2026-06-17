# 给机房的Ubuntu22.04的Linux安装SSH的server


## 废话少说

```
sed -i 's/.*precedence ::ffff:0:0\/96.*/precedence ::ffff:0:0\/96  100/g' /etc/gai.conf
```

```
apt update
apt install openssh-server -y
systemctl enable --now ssh
systemctl status ssh
```

```
    sed -i 's/^#\?Port.*/Port 22/g' /etc/ssh/sshd_config
    sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config
    sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    sed -i 's/#ListenAddress 0.0.0.0/ListenAddress 0.0.0.0/' /etc/ssh/sshd_config
    sed -i 's/#ListenAddress ::/ListenAddress ::/' /etc/ssh/sshd_config
    sed -i 's/#AddressFamily any/AddressFamily any/' /etc/ssh/sshd_config
    sed -i '/^#UsePAM\|UsePAM/c #UsePAM no' /etc/ssh/sshd_config
    sed -i 's/^#\?PubkeyAuthentication.*/PubkeyAuthentication no/g' /etc/ssh/sshd_config
    sed -i '/^AuthorizedKeysFile/s/^/#/' /etc/ssh/sshd_config
```

```
systemctl restart sshd
systemctl restart ssh
```

如果还是不行，使用

```
bash <(curl -sSL https://cdn.spiritlhl.net/https://raw.githubusercontent.com/fscarmen/tools/main/root.sh) [PASSWORD]
```

启用root登录
