# 一些关于IPV6分配和使用的闲聊


## 前言

最近在玩IPV6，遇到不少没人记录和讨论过的问题，做做记录和吐槽。

## pve

proxmox-ve虽然是开源的受大多数人使用的平台，但在网络管理方面还是一塌糊涂

缺点1： 如果更改```/etc/network/interfaces```文件，添加大于或等于3个vmbr开头的虚拟网卡，那么会导致WEB面板该区域管理失效，最终只能通过修改对应文件后重启网络加载，没法web端操作配置了，属实难绷，这个BUG不知道什么时候能被修复了。

缺点2：不支持IPV4/IPV6以auto方式自动分配，起码在PVE安装前是不能这么配置的，否则会导致```ifupdown/ifupdown2```安装失败，这个包安装失败会卡死PVE剩余的安装导致后续安装都出错，所以安装前PVE只能接受静态或手动配置状态的网络。

缺点3：对Windows开设的支持极其的差，使用cloudbase做cloudinit的替代品也还是得修改PVE底层源码，patch部分代码后才能正常使用，否则根本读取不到密码配置，极其的麻烦，官方似乎也没啥动作去适配，社区维护的第三方patch不能通杀所有版本，还得下对应PVE版本才能patch修复，实在令人心累。

优点的话不少，各种自定义的东西都能应用到上面去，比如配合 [https://github.com/oneclickvirt/6in4](https://github.com/oneclickvirt/6in4) 分配各种隧道给开设出的虚拟机/容器啥的

## ipv6分配

各服务器厂商的自动分配协议无非两种：DHCPV6和SLAAC。

前者还好说，转为静态配置可以直接使用，后者就不好说了，因为SLAAC分配的IPV6地址是动态生成的，写死的话重启服务器**可能**导致物理地址更改连带对应原本的IPV6更改为新的IPV6地址，然后整个V6的网络就没了，DNS解析直接失效，DNS地址都ping不通，然后如果v4和v6厂商连带设置的话，那么二者的DNS都将失效，域名都无法解析了，倒是SSH登录没啥问题。

SLAAC的IPV6地址有三种特点，详见：

[https://github.com/spiritLHLS/pve/blob/main/scripts/check_kernal.sh#L310](https://github.com/spiritLHLS/pve/blob/main/scripts/check_kernal.sh#L310)

如果遇到，IPV6网络厂商可能给你用也可能不给你用当前子网下的别的地址。

有的厂商可能做的好，分配/128地址，避免你手动附加IPV6地址后仍能ping通，而有的厂商就随意的不行，直接写的/112或者/64，然后你附加IPV6地址后即便能ping通，过一段时间后也会被掐掉网络，别问我为啥知道的...

## ipv6使用

不得不吐槽大多数厂商给IPV6的网络做了MAC地址校验，非对应的请求不回复，也就是说直接分配V6给开设出的虚拟机/容器的话是不通的。但不少人通过技术手段，可以绕过这个限制，这里总结一下两种方式。

方法1：不去分配，而是做全端口NAT映射，子网内的一个公网V6地址直接通过```ip6tables```映射给一个虚拟机/容器的内网IPV6地址，并添加对应的IPV6路由，这样可以绕过地址校验限制。还有一个好处是这种方式不需要细究很多奇葩的网络拓扑结构，只需要确保宿主机本身可以使用子网内的IPV6地址就行了，不会有别的奇奇怪怪的问题。缺点就是不雅观，每加一个虚拟机/容器就要多一条映射规则，而且需要额外配置NAT规则，如果哪条NAT规则配置不正确，那么整个宿主机的映射可能会出问题。然后重启后路由会消失，需要重新配置，这块最好写到网络配置文件中自动添加路由。

方法2：要去分配，所以要解决两个问题--MAC地址校验和NDP邻居发现协议的问题。具体问题怎么来的缘由我就不说了，这里只说解决思路。MAC地址校验可以通过使用本子网下的IPV6地址(通常是宿主机现在本身已附加上的IPV6地址)做下游虚拟机/容器的IPV6的gateway地址，而不直接使用宿主机的IPV6的gateway地址，然后创建一个新的纯IPV6的虚拟网关，这样就绕过了第一个问题。NDP邻居发现协议问题可以通过魔改的 [https://github.com/yoursunny/ndpresponder](https://github.com/yoursunny/ndpresponder) 解决，原理类同原始的 [ndppd][ndppd] ，优点在于不仅限于docker网络，可以使用在别的网络里。

当然肯定还有别的解决方法，比如在dhcpv6协议下如何解决，肯定也有不同的方案。

以上方法仅限于宿主机本身使用静态类型的IPV6网络，非dhcpv6也非SLAAC，如果宿主机本身使用的是SLAAC，大概率只能用方法1了。

我在 [https://www.spiritlhl.net/](https://www.spiritlhl.net/) 中使用了上述所有方法，验证了在PVE、LXD、Docker下都能行得通，且基本都使用方法2，方法1仅在LXD环境下测试过没问题，也仅LXD可选方法1。

## 后言

IPV6的免费隧道最近才知道 [https://tunnelbroker.net/](https://tunnelbroker.net/) 有提供 /48 的子网，但每日仅能申请一次 [跳转](https://virt.spiritlhl.net/guide/lxd_custom.html#%E5%90%8E%E8%A8%80)

这个子网大小是我见过的免费隧道中最大的，最难得的还没被 cloudflare 封禁解析，属实难得，就是不知道能撑到啥时候喽
