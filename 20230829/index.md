# 为Docker配置启用IPV6网络并配置给容器自动分配IPV6地址(2023最新)


## 前言

观望了全网的Docker启用IPV6的方法，要么是Docker版本更替法子不通了，要么是没说明一些前置条件的细节，导致方法也用不了，所以这里记录一下我走通的方法，一个兼容高低版本Docker和不同网络环境的方法

## 环境

常见的方法是给Docker增加```/etc/docker/daemon.json```配置项，如

```
{
  "ipv6": true,
  "fixed-cidr-v6": "这里填IPV6的Gateway地址/子网掩码"
}
```

但这种方法是有问题的，并不是所有宿主机都支持这么配置然后IPV6附加上去就通了的

宿主机的Gateway可能是公网地址和私网地址都有的，或者是要么只有公网，要么只有私网

对于只有私网的ipv6的Gateway当然可以如上配置，但只有公网的ipv6的Gateway就不能这么配置了，理论上需要iptables进行映射，都有的情况又得另外写一篇了

以上方法都很麻烦，因为即便配置好了网络，实际你仍旧需要手动一一附加IPV6地址给容器，这就很麻烦，下面介绍一下我的方法

## 使用 ndpresponder + radvd + 新建bridge 实现自动分配地址

如果嫌弃手动方法麻烦，可以移步 [https://virt.spiritlhl.net/guide/docker_precheck.html](https://virt.spiritlhl.net/guide/docker_precheck.html) 有写好自动化设置的方法，以下是手动设置的方法，仅作记录

以下示例已假设你物理网卡是eth0，是别的自行替换名字

### 前期准备

前期中的前期是先查看本机使用什么管理网络，执行

```shell
systemctl is-active systemd-networkd
```

和

```shell
systemctl is-active networking
```

看看属于哪种情况，如果是前者active，你需要切换本机使用ifupdown或者ifupdown2管理网络，如果是后者，则不需要切换网络管理程序，直接进行后续操作即可。

首先是确认一下IPV6的子网大小，宿主机需要至少/64大小的子网，因为后面需要划分出/80大小的子网使其自动附加到容器上，当然这不是绝对的，你可以划分更大的子网或者宿主机的子网更小也没问题，唯一的原则就是后续划分的子网中，不能包含宿主机的IPV6地址和IPV6的Gateway，这个是重点。

然后就是判断宿主机的IPV6的Gateway究竟是公网还是私网，亦或者是两者都有，执行以下命令查询

```shell
ip -6 route show | awk '/default via/{print $3}'
```

如果只有一行fe80开头的就是只有私网gateway

如果是没有fe80开头的行但有别的开头的ipv6地址，就是只有公网gateway

如果有两行甚至多行，fe80开头的行也有，非fe80开头的行也有，那就是公私网的gateway都有

如果你是只有公网的gateway，先执行

```
ip -6 addr show dev eth0 | awk '/inet6 fe80/ {print $2}'
```

查询fe80的地址，然后需要你手动修改```/etc/network/interfaces```文件新增一行在末尾

```
up ip addr del 这里填你查询到的地址 dev eth0
```

如果你是公私网gateway都有，那么```/etc/network/interfaces```文件的ipv6的gateway最好用公网的v6地址，然后也如同只有公网的gateway那样新增一行上面的内容

修改过文件后最好使用```chattr```命令锁死文件只读然后重启系统，以加载网络修改

如果是只有私网的gateway，那么```/etc/network/interfaces```文件无需修改，也无需重启系统

### 安装radvd和ndpresponder并新建bridge

radvd正常只需要通过对应系统的包管理器下载就行了

如apt的直接```apt install radvd -y```，yum的直接```yum install radvd -y```，别的类同

然后修改配置文件```/etc/radvd.conf```，覆写如下

```
interface eth0 {
  AdvSendAdvert on;
  MinRtrAdvInterval 3;
  MaxRtrAdvInterval 10;
  prefix 这里写你IPV6的地址除去最后一个冒号后的内容但保留冒号/这里写你IPV6的子网掩码大小 {
    AdvOnLink on;
    AdvAutonomous on;
    AdvRouterAddr on;
  };
};
```

然后就是创建一个新bridge命名为ipv6_net

```
docker network create --ipv6 --subnet=172.26.0.0/16 --subnet=这里填你之前划分的/80子网带子网掩码的地址 ipv6_net
```

然后是ndpresponder的安装，ndpresponder是用来处理IPV6的NDP协议的，是ndppd的修改版本，所以安装方式不同，无法通过官方的包管理器安装。

这里我有编译好对应每个架构的docker镜像，自取docker镜像安装

x86的如下

```
docker run -d \
    --restart always --cpus 0.02 --memory 64M \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    --cap-drop=ALL --cap-add=NET_RAW --cap-add=NET_ADMIN \
    --network host --name ndpresponder \
    spiritlhl/ndpresponder_x86 -i eth0 -N ipv6_net
```

arm的如下

```
docker run -d \
    --restart always --cpus 0.02 --memory 64M \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    --cap-drop=ALL --cap-add=NET_RAW --cap-add=NET_ADMIN \
    --network host --name ndpresponder \
    spiritlhl/ndpresponder_aarch64 -i eth0 -N ipv6_net
```

然后你可以使用以下命令开设容器看看是否IPV6镜像已经通畅了

```shell
docker run --network=ipv6_net --rm -it busybox ping -6 -c4 ipv6.ip.sb
```

## 总结

以上方式简单易懂，适配所有可能的IPV6的gateway配置情况，当然如果你嫌弃手动修改麻烦，还是推荐使用

[https://virt.spiritlhl.net/guide/docker_precheck.html](https://virt.spiritlhl.net/guide/docker_precheck.html) 

有写好自动化设置的方法，自动分辨对应情况进行修改，自己看对应说明使用即可

相关仓库 [https://github.com/spiritLHLS/docker](https://github.com/spiritLHLS/docker)
