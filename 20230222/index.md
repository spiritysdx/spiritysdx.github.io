# 将WIN系统DD为Linux系统


### 前言

该方法的前提：

商家使用PVE虚拟化WIN服务器

WIN服务器可配置使用Network进行启动

启动时可使用NO VNC操控IPXE命令

已知符合条件的服务器：[NETFRONT](https://hosting.netfront.net/aff.php?aff=126&pid=67)

配置：20Mbps Windows 不限流量 Unlimited Traffic (4C+4G+128G+1IP) 香港C区

### 步骤

#### 1

更改```Boot Order```选项为如下形式

```
Device 1: Network
Device 2: Disk
Device 3: None
```

![图片](https://user-images.githubusercontent.com/103393591/220626080-577842ff-f113-4bc1-aada-481526746467.png)

![图片](https://user-images.githubusercontent.com/103393591/220626165-b8d3d2a5-e727-49a7-8368-e15047fe6201.png)

#### 2

然后记录下IPV4地址和GATEWAY地址

![图片](https://user-images.githubusercontent.com/103393591/220626413-be618aed-c625-4132-bd5b-01a3304b13a9.png)

打开no VNC 等待提示使用CTRL+B键打开IPXE命令窗口

![图片](https://user-images.githubusercontent.com/103393591/220627018-6c0a089e-b2f2-4dfc-8ca7-f4993b872817.png)

输入以下命令

```
set net0/ip 你的IPV4地址
set net0/netmask 255.255.255.0  # 你的SUBNET MASK，一般不用改
set net0/gateway 你的gateway地址
set dns 8.8.8.8
ifopen net0
chain -a http://boot.netboot.xyz # 或 chain --autofree http://boot.netboot.xyz
```

![图片](https://user-images.githubusercontent.com/103393591/220627596-cfdb17e6-7bcb-4a13-9a8a-0bbd2fff607e.png)

这些命令的解释(不看也没问题)：https://netboot.xyz/docs/booting/ipxe/

#### 3

回车后等待一段时间

![图片](https://user-images.githubusercontent.com/103393591/220627786-55f41d6e-6afd-41e9-a608-bf38051d7e45.png)

![图片](https://user-images.githubusercontent.com/103393591/220627872-ac79e1e9-e8c0-4b5d-bb14-225e15b1dcc0.png)

选择```Linux Network Installs (64-bit)```

#### 4

然后选择需要的系统安装

![图片](https://user-images.githubusercontent.com/103393591/220627984-53f3a2e3-92fc-4891-ab22-bcdfe6084913.png)

![图片](https://user-images.githubusercontent.com/103393591/220628151-d99cb18d-63e7-43f0-9ce8-9972796ee554.png)

![图片](https://user-images.githubusercontent.com/103393591/220628231-528a1cce-0635-46ca-b104-6360ef5b9df3.png)

我习惯debian的debian11中的```Expert install```

#### 5

后续就是按照地域选择语言，选择使用美式英语键盘等，直到下图提示

![图片](https://user-images.githubusercontent.com/103393591/220628584-10103de3-b04f-4fdb-abe1-63abfa99b9bc.png)

不要使用DHCP自动配置网络，因为可能会出现配置失败，此时这个选择要选择NO进行手动配置

配置时需要依次填入

IPV4地址：

![图片](https://user-images.githubusercontent.com/103393591/220628999-12c46bea-030e-45e4-88ef-5e628a92ce07.png)

SUBNET MASK(netmask)：

![图片](https://user-images.githubusercontent.com/103393591/220629111-17619505-0caf-4801-ac7c-9f706c0cd03a.png)

GATEWAY：

![图片](https://user-images.githubusercontent.com/103393591/220629345-e15df08d-d11a-44dd-80f0-916597fee2d2.png)

nameserver：

![图片](https://user-images.githubusercontent.com/103393591/220629545-904846cf-3af2-4c80-99d3-09ffdaf898fc.png)

其中nameserver使用```8.8.8.8```即可，其他后续会自动补全，比如hostname什么的，如果不自动补全就是有问题的了，需要回退上一步重新设置network(重新设置nameserver)

#### 6

![图片](https://user-images.githubusercontent.com/103393591/220629975-9c9c1c1b-6b40-47af-a2b0-2538f7aa6f63.png)

```installer components```那块不要选择任何组件，直接按TAB跳到continue按键继续

#### 7

后续基本就是全程回车默认即可，设置root密码和新用户以及新用户密码记得记一下，否则后续难改得重装系统了

![图片](https://user-images.githubusercontent.com/103393591/220630568-986a62d9-f4e9-4eb6-b397-fa8c85bd5747.png)

后面disk分配的时候，默认回车到最后会选择是否要写入更改，此时需要选择yes写入，否则会一直停留在该界面

#### 8

![图片](https://user-images.githubusercontent.com/103393591/220631078-95b8b232-9e85-48e0-847a-a9b78194806f.png)

选择镜像时使用```linux-image-cloud-amd64```而不是```linux-image-amd64```以节省空间

#### 9

![图片](https://user-images.githubusercontent.com/103393591/220631195-258c1149-7c37-49d7-8807-d186f23efbf2.png)

选择镜像完毕后需要选择```initrd```，推荐选择第二个选项```targted```开头的选项，以减少空间占用

#### 10

修改前的显示如下：

![图片](https://user-images.githubusercontent.com/103393591/220631809-0cc6678f-9225-40fd-9398-8cdc46018488.png)

继续安装后面需要选择```Software selection```，选项列表中的第一第二个选项

```
Debian desktop environment
GNOME
```

的选中需要取消掉，避免安装桌面环境(按空格移除选中)，修改后：

![图片](https://user-images.githubusercontent.com/103393591/220631882-4a56354f-7dd8-4bc0-b859-784dbe932947.png)

然后将```SSH server```选中，不然进去系统你还得自己装SSH，这里选了就不用再自己装了，然后按TAB跳到continue按键继续

![图片](https://user-images.githubusercontent.com/103393591/220632118-cd4d3b9a-d9b8-4273-92cc-4b93376842cb.png)

#### 11

![图片](https://user-images.githubusercontent.com/103393591/220632593-72508a7d-01af-42e1-b02c-b7086e58ff74.png)

最后在选择```Install the GRUB boot loader```的时候需要在选项列表选择第二个选项，不要默认，选择使用类如```/dev/sda```开头的选项，然后选择```Yes```

![图片](https://user-images.githubusercontent.com/103393591/220632649-ef0e5ead-d388-4a53-a6be-13d66ef7e00d.png)

安装完毕后会默重启系统

#### 12

不要使用重启后的系统，先更改这里

![图片](https://user-images.githubusercontent.com/103393591/220632824-e3210494-3b27-419a-8f3b-98854983b57c.png)

更改```Boot Order```选项为如下形式

```
Device 1: Disk
Device 2: None
Device 3: None
```

然后```Shutdown```再```Boot```

重启后会出现选项选择如何启动，默认回车GRUB启动就行

![图片](https://user-images.githubusercontent.com/103393591/220633361-5cdff1e3-70b1-42f1-8a3a-481132662354.png)

![图片](https://user-images.githubusercontent.com/103393591/220633418-b0af4315-262e-48cf-86f9-99f8b6b30d25.png)

![图片](https://user-images.githubusercontent.com/103393591/220633451-5201e712-75f1-44ed-bd5e-4275bbec78ce.png)

#### 13

登陆后依次执行

```bash
apt update
apt install vim -y
```

进去后由于未开启SSH使用远程root登陆，所以依然需要```NO VNC```进去后，修改```/etc/ssh/sshd_config```

```
vim /etc/ssh/sshd_config
```

修改前：

![图片](https://user-images.githubusercontent.com/103393591/220634141-8d7dfd5b-f485-4456-91f4-78784ad70591.png)

修改后：

![图片](https://user-images.githubusercontent.com/103393591/220634201-c807233f-d610-4575-bfc4-82889dd196cc.png)

![图片](https://user-images.githubusercontent.com/103393591/220635319-7286401e-47d6-4ce9-9a03-f07a92c28131.png)

将```#PermitRootLogin prohibit-password```修改为```PermitRootLogin yes```，记得去除开头的#号

![图片](https://user-images.githubusercontent.com/103393591/220634362-f9252ab9-5b0f-451e-9b7f-18769e9ee24a.png)

最后退出编辑保存的使用输入```service sshd restart```和```service ssh restart```重启SSH服务

### 后言

这样捣鼓之后系统就DD完毕且可以在外面使用SSH使用root登陆了
