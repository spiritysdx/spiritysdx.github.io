# 给轻量应用腾讯云服务器加上IPV6地址隧道


### 1

进入页面[https://tunnelbroker.net/](https://tunnelbroker.net/)注册

注意需要“魔法”，而且不能使用yandex邮箱作为注册信息。

![](https://img-blog.csdnimg.cn/20210115091650664.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)

### 2

注册完毕后登录,点击左侧的 create regular tunnel

![](https://img-blog.csdnimg.cn/20210115091650643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)

### 3

右上角框内填入腾讯云的IPV4的外网地址

![](https://img-blog.csdnimg.cn/20210115091650701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)

同时选择离你的服务器的地址比较接近的，比如说大陆的服务器选择香港地址。

![](https://img-blog.csdnimg.cn/20210115091650644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)

### 4

在隧道详细信息中,点击Example Configuration

![](https://img-blog.csdnimg.cn/20210115091650657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210115091650683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)

### 5

选择完毕后稍等片刻,下面我们以CentOS系统为例(其他系统同理),把下图中所指的地方改为你自己服务器的内网IP

![](https://img-blog.csdnimg.cn/20210115091650645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)

这个内网地址在腾讯云服务器的网络信息中有写，注意在 “local” 名字后的IPV4地址改为内网地址

修改完后，需要在服务器的/etc/network/interfaces 文件内粘贴修改后的内容

如果是宝塔面板，直接在文件管理中打开该文件修改即可

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/VL@E7L224B%7BB$I7XK(~5)FK.png)

当然如果没有宝塔面板，可以使用vim工具修改

vim修改步骤：

ssh链接上服务器后，（Ubuntu/Debian）输入sudo -i进入管理员模式，（centos）不需要这一步。

进入后，输入

```
vim /etc/network/interfaces
```

打开文件

按i键，进入插入模式

最后一行插入之前修改好的内容

然后按左上角esc键退出编辑模式

最后输入（英文冒号不是中文冒号）

```
:wq
```

按下回车，该文件就修改完毕并退出文件了。

### 6

此时输入 ifconfig 回车

这个地址显示出来就是绑定成功了

![](https://img-blog.csdnimg.cn/20210115091650646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zNzc1ODI5Nw==,size_16,color_FFFFFF,t_70)


## 后言


推一波项目合集：(仓库说明就是合集)

[https://github.com/spiritLHL/Hang-up-items](https://github.com/spiritLHL/Hang-up-items)

