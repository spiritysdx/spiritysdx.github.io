# 给机房的windows11系统安装Ubuntu22.04的Linux双系统


## 前言

前置条件以及一些碎碎念：

至少两块硬盘，一块硬盘已安装Windows11，另一块硬盘用于安装```Ubuntu22.04```，两块硬盘保证系统不会互相影响出奇怪的问题

为什么选择```Ubuntu22.04```？因为前不久刚发布了新的LTS版本24.04，不敢追新怕出现什么奇葩问题没有先验的经验，所以还是使用第二新的LTS版本22.04保险一些，保证系统稳定易于维护。

```Ubuntu22.04```下载地址(4.7GB-下载到你的本地，不是U盘中)：

[https://releases.ubuntu.com/22.04/](https://releases.ubuntu.com/22.04/)



一个读写速率比较好的U盘，用于制作启动盘，由于启动盘需要使用```rufus```进行启动盘制作，所以需要U盘格式化，需要确保U盘制作前为空U盘

```rufus```下载地址(下载到你的本地，不是U盘中)：

[https://rufus.ie/zh/](https://rufus.ie/zh/)

或

[https://github.com/pbatard/rufus](https://github.com/pbatard/rufus)


安装过程中需要选择引导方式，两个选择：```Windows Boot Manager```或```Grub```，前者需要windows下面添加引导条目，后者需要手动分区，在安装后添加grub引导条目。

不管选择何种引导方式，都不要相信```和*****共存```的选项，因为是装的双系统，需要保留WIN系统的相关分区，Ubuntu在安装时会提示```和Windows Boot Manager共存```，如果选择该项，那么硬盘上就真的只剩```Ubuntu```相关分区和```Windows Boot Manager```了，WIN系统相关分区直接没了。

为了保险，推荐安装双系统前整机进入```BIOS```中```GOST```备份(睡觉前挂上，运行非常慢)。然后安装时手动进行分区(不要相信自动分区的鬼话，那是给纯净的机器用的，不是给双系统用的)，然后在GRUB手写一个WIN11的启动项。(不要瞎转换硬盘格式)

这里推荐使用[https://linux.cn/article-14699-1.html](https://linux.cn/article-14699-1.html)作为前置教程，进行相关流程了解。

## 制作U盘引导盘

![1](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240508224645.png)

选择使用DD写入U盘

![2](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240508224651.png)

点击开始后等待几分钟即可，DD写入成功后，正常安全退出U盘即可。(启动盘制作完成，回退回普通U盘的方法后续会介绍)

## 切分分区

按下 ```Windows + R``` 组合键来打开磁盘管理器实用程序

输入 ```diskmgmt.msc``` 打开磁盘管理器，如下图

![3](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205835.jpg)

下面可选从已有的盘中分一部分出来用，或者选择整个盘拿来用(注意，这里千万不能使用C盘，因为C盘是windows系统盘，如果C盘被Linux拿来用，那么windows系统很可能就无法正常使用了，所以千万不能使用C盘)

1.选择一个卷(里面的数据的全部不需要了)进行```压缩卷```处理，如下图

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205839.jpg)

划分25G硬盘给Linux使用

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205918.jpg)

2.如果这个盘你想要整个盘用作Linux使用，那么直接点击```删除卷```即可，如下图

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059181.jpg)

由于这次我安装的工作站硬盘很大，4TSATA+2TSSD，我选择将整个2T大小的D盘作为Linux系统使用，所以我使用了方法2。

## 引导安装

上述两个方法选其一操作，操作完毕后就有了可用做Linux使用的空间，现在将可启动 USB 驱动器插入到你的电脑 ，重新启动你的电脑。启动过程中狂按```F12```进入 BIOS 设置修改启动优先级，来使 USB 驱动器成为第一优先级，保存 BIOS 更改并继续启动。(我实际测试中工作站是直接显示可选使用哪个UEFI项启动，选择对应U盘的UEFI项启动即可，如下图)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205920.jpg)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205932.jpg)

选择USB对应的UEFI启动后，进入GRUB选择```Try or Install Ubuntu```按回车

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059201.jpg)

Ubuntu 22.04 将开始加载，这最多需要一分钟时间进入Ubuntu的图形界面。

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059211.jpg)

更改引导的语言为简体中文，然后点击```安装Ubuntu```

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205922.jpg)

此时会进入选择键盘设置的界面，可如下图选英文键盘，也可以如后一个图一样选中文键盘，没什么区别

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205931.jpg)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205933.jpg)

选完点继续，进入安装选择，勾选如下图，再点继续

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059311.jpg)

## 手动划分分区

此时下图是重点中的重点，一点要选择```其他选项(Something else)```，千万不要选择共存(共存就擦除WIN系统了)，这个千万别勾选错，然后点继续

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059312.jpg)

这里可以看到空闲区间，如果你没有看到空闲区间，那么你需要在这个页面上勾选你之前选择的盘对应的路径，然后点减号删除分区(重点：一定要通过之前分区的大小和空闲区间的大小来判断，哪个分区是你之前选择做Linux系统的，千万别删错删了WIN系统盘，不然就救不回来了，切记切记)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059331.jpg)

然后就是点击那个空闲分区，点加号，手动划分分区

针对 Ubuntu 22.04 ，将创建下面的分区(注意创建的顺序，不要搞错了)：

```
/boot        –        1024MB - 1GB
/home        –        10240MB - 10GB
交换分区      –        65535MB - 64GB - 因为我实际物理内存64G，SWAP开等大的即可
EFI          –        300 MB
/            –        空闲分区余下的空间
```

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205934.jpg)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205935.jpg)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205936.jpg)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059361.jpg)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205937.jpg)

创建后如下，然后点```现在安装```

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205938.jpg)

遇到下面的界面，点```继续```

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205939.jpg)

过一会就要选择地区，这里填```shanghai```，或者直接鼠标点中国地图那就行，然后点```继续```

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059391.jpg)

再过一会就是填写登录信息，填写完毕务必记录登录的密码和用户名，因为后续我需要长期使用，所以勾选了```自动登录```，不勾选自动登录，勾选```登陆时需要密码```也可，然后点```继续```

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205940.jpg)

后续安装进度条跑满大概需要10~30分钟，然后出现下图提示

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059401.jpg)

点```现在重启```即可，进入重启且黑屏后，拔掉U盘即可，然后启动时应该以装好的Ubuntu的GRUB界面引导启动如下

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20240509205941.jpg)

此时按回车即可启动Ubuntu系统

如果要切换回windows11系统，GRUB界面选择```Windows Boot Manager```，然后按回车即可
`

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_202405092059411.jpg)

在WIN系统中可见D盘不可获取大小，因为我已经将整个D盘分配给Linux使用了

## 更改启动项

实际机房的工作站默认给WIN系统配了第一优先级，我需要修改为Ubuntu系统为第一优先级，方法有两个：

1. 启动机器时，狂按```F1```或者```F2```按键，进入BIOS界面，设置```主要启动顺序```

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/photo_2024-05-09_23-13-59.jpg)

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/photo_2024-05-09_23-14-12.jpg)

2. 在WIN系统中使用[easyuefi](https://easyuefi.com)直接设置启动顺序

![](https://raw.githubusercontent.com/spiritysdx/images/main/20240508/photo_2024-05-09_23-17-03.jpg)

由于机房使用PXE部署了```噢易```系统，所以上述方法修改启动项都是无用的，千万不能再启动WIN11了，否则就永远也无法进入Ubuntu系统了，只能重新安装Ubuntu系统

## 后言

后续将教授怎么使用Ubuntu系统通过LXD开设LXC容器共享宿主机资源(含GPU资源)、内穿机房端口至于公网IPV4上、部署Github的Runner等内容
