# 在Pycharm上连接远程虚拟环境进行使用


## 前言

相信通过前面教程，大部分问题都解决了，目前就剩一个Pycharm怎么连接远程的LXC容器运行Python项目的问题了。

(为什么不用VSCODE？因为它没有官方支持的远程开发/SSH开发的功能，只有相关的第三方插件实现了类似的功能但并不好用，所以我选择了Pycharm)

(还有一个原因是我有开源证书，所以Pycharm专业版可以免费使用)

## 步骤

1. 打开Pycharm，点击菜单栏的`File`，选择`Settings`

2. 在弹出的窗口中，选择`Project:xxx`，然后选择`Project Interpreter`，然后选择`Add Interpreter`，在下拉窗口中选择`On SSH`

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-19-46.png)

3. 按照下面的图示填写信息即可

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-20-53.png)

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-21-11.png)

4. 下面这一步得等一会，它要测试远程连接和环境

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-21-53.png)

5. 这里一定要选择`Existing`，点那三个点自己增加已有的conda解释器，而不是pycharm导入解释器或者是容器本地的解释器，我需要使用虚拟环境。

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-27-24.png)

6. 这里输入conda虚拟环境解释器的路径，在容器内可使用以下命令查询

```
(base) root@user2:~# command -v python
/root/miniconda3/bin/python
```

我在base环境中查询到解释器路径是```/root/miniconda3/bin/python```，如果你也是使用 [https://www.spiritysdx.top/20240513/](https://www.spiritysdx.top/20240513/) 配置的LXD多用户容器，那么路径应该和我一致。

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-28-02.png)

7. 这里是修改远程容器保存项目代码的路径，我选择保存在容器内的```/root/item1```文件夹里，你可以自行修改。

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-28-30.png)

8. 全部OK点击完毕后，在解释器选择页面会看到类似如下的输出。

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-29-40.png)

9. 如果想要每次启动Pycharm自动登录到容器，那么需要在这里配置记住密码，每次打开Pycharm自动登录SSH。

建议参考： [https://blog.csdn.net/pianchengxiaobai/article/details/136660228](https://blog.csdn.net/pianchengxiaobai/article/details/136660228)

(由于我的Pycharm是开源证书的专业版，所以有该功能，社区版本未尝试不知道有没有)

10. 配置完毕后在这里进入容器的SSH界面/Conda的Python界面，你可以在SSH上面执行```ls```查看是不是有你之前映射的项目文件夹。

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/image_2024-05-14_16-42-11.png)

我的执行结果是

```
Last login: Tue May 14 07:56:28 2024 from 127.0.0.1
(base) root@user2:~# ls
item1  miniconda3  Miniconda3-latest-Linux-x86_64.sh  nezha.sh  NVIDIA-Linux-x86_64-535.171.04.run  sharefile  snap
```

确实有```item1```这个文件夹了。

## 后言

注意这里的连接速度受跳板机到机房宿主机的网络的影响，国内机房的宿主机开设的LXD多用户容器务必使用境内/香港服务器做跳板机，否则你的网络延迟会非常大，无法使用。

本人推荐使用腾讯云的境内服务器[跳转](https://cloud.tencent.com/act/cps/redirect?redirect=11529&cps_key=c57aeee1ac0c5493f6718c40421affcf)，阿里云[跳转](https://www.aliyun.com/?source=5176.11533457&userCode=io97dzbb)的同价位的服务器CPU会比较弱一些，在多用户使用的时候可能出现CPU占用过高的情况。

(北京/上海/广州比较好，香港也还行，这些地方有骨干网的POP点，延迟会低一些)

下一篇讲解如何给机房的LXD容器配置跳板机进行连接。
