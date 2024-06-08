# conda使用GPU时的一些陷阱


## conda默认的python环境版本过高，需要降级默认的Python环境

假设原始环境是Python12的环境

直接使用

```
conda install python==3.11
```

类似上述方法指定python版本下载，注意这个命令要在conda的虚拟环境中执行才会替换当前python版本。

## 环境存在冲突，但非报错提示，需要手动指定环境

例子：

安装了pytorch，但安装了pytorch-geometric后，pytorch的版本从GPU版本变成CPU版本了，这是怎么回事呢？

查看官网： [https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html](https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html)

查看说明才知道pyg的cuda版本在conda中是默认识别不到的，如果使用：

```
conda install pyg -c pyg
```

下载的是仅CPU版本，所以安装中会更换Pytorch的版本，导致Pytorch的版本从GPU版本变成仅CPU版本了。

解决办法：指定cu进行cuda版本的安装，不要使用conda的默认安装。

```
conda install pyg=*=*cu* -c pyg
```

这样安装得到的版本就是GPU版本了，不会影响pytorch的版本。

由此可见有些第三方包在conda中是默认找不到GPU版本的(或者说conda的默认安装环境配置中gpu版本不是首选)，需要手动指定cu版本安装。

## python的第三方包调用了被屏蔽的网址

https://github.com/mzz2017/gg

安装后使用对应命令再调用python解释器
