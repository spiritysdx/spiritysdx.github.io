# nvidia-smi被自动升级无法与GPU通信了怎么办


## 问题

如题目所说，这里贴个报错

```
nvidia-smi
Failed to initialize NVML: Driver/library version mismatch
NVML library version: 535.183
```

原先的版本是

```
 NVIDIA-SMI 535.171.04             Driver Version: 535.171.04   CUDA Version: 12.2 
```

## 修复方案

### 下载官方驱动

打开官方网站：[https://www.nvidia.cn/Download/Find.aspx?lang=cn](https://www.nvidia.cn/Download/Find.aspx?lang=cn)

按照你的显卡版本进行选择，我的配置如下，你的与我一般不一致

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/circle_screenshot_1_%E5%AE%98%E6%96%B9%E9%AB%98%E7%BA%A7%E9%A9%B1%E5%8A%A8%E6%90%9C%E7%B4%A2%20NVIDIA(1).png)

然后点击```搜索```进行搜索，我搜索结果如下

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/(AMD64_EM64T)%20NVIDIA.png)

出现下面这个界面后，别急着点击下载

![](https://cdn.spiritlhl.net/https://raw.githubusercontent.com/spiritysdx/images/main/20240513/circle_screenshot_1_unknown.png)

**右键**点击```同意并开始下载```复制下载链接，在**命令行**中使用```wget```下载

### 卸载旧的驱动

```
nvidia-uninstall
```

注意：如果此前是用官方*.run文件安装的，需要通过官方卸载程序卸载；

或：

```
sudo apt-get purge nvidia-*
```

如果之前是通过apt安装的，则通过apt-get卸载干净；如果此前是通过*.run文件安装的，该命令无法有效卸载。

### 安装编译所需的必要包

```
sudo apt install gcc-12 g++-12 make -y
sudo apt install pkg-config libglvnd-dev -y
```

更新默认 GCC 版本 - 这是因为我这个驱动是gcc12编译的，所以需要对应下载和切换版本，如果你的驱动查看报错显示是别的版本，需要你自行下载和切换版本

```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 10
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 20
sudo update-alternatives --config gcc
```

显示

```
有 2 个候选项可用于替换 gcc (提供 /usr/bin/gcc)。

  选择       路径           优先级  状态
------------------------------------------------------------
* 0            /usr/bin/gcc-12   20        自动模式
  1            /usr/bin/gcc-11   10        手动模式
  2            /usr/bin/gcc-12   20        手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：
```

上面我已经选好了

### 停止可能占用的进程

```
sudo systemctl isolate multi-user.target
sudo systemctl stop nvidia-persistenced
sudo rmmod nvidia_uvm
sudo rmmod nvidia_drm
sudo rmmod nvidia_modeset
sudo rmmod nvidia
```

检查内核

```
lsmod | grep nvidia
```

### 安装驱动

然后执行(我下到的是```NVIDIA-Linux-x86_64-535.171.04.run```文件)

在宿主机中：

```
chmod 777 NVIDIA-Linux-x86_64-535.171.04.run
sudo ./NVIDIA-Linux-x86_64-535.171.04.run
```

在LXD容器中

```
bash NVIDIA-Linux-x86_64-535.171.04.run --no-kernel-module
```

安装选项都选择```continue```即可，安装好显卡驱动后执行```nvidia-smi```查看显卡驱动是否成功与GPU通信。

如果在**LXD容器中不是在宿主机中**此时显示有问题```Failed to initialize NVML```，重启容器可解决。

## 长久解决方案

即使这次驱动被回退了，还是有可能被ubuntu官方更新连带更新。

查看设置

```
cat /etc/apt/apt.conf.d/10periodic
```

```
APT::Periodic::Update-Package-Lists "7";
APT::Periodic::Download-Upgradeable-Packages "0";
APT::Periodic::AutocleanInterval "0";
APT::Periodic::Unattended-Upgrade "1";
```

修改为全是0并保存文件避免自动升级包。

### 设置锁死包版本

命令行执行下面的命令

#### 锁死nvidia相关包

```shell
packages=(
    "libnvidia-cfg1-535:amd64"
    "libnvidia-common-535"
    "libnvidia-compute-535:amd64"
    "libnvidia-compute-535:i386"
    "libnvidia-decode-535:amd64"
    "libnvidia-decode-535:i386"
    "libnvidia-encode-535:amd64"
    "libnvidia-encode-535:i386"
    "libnvidia-extra-535:amd64"
    "libnvidia-fbc1-535:amd64"
    "libnvidia-fbc1-535:i386"
    "libnvidia-gl-535:amd64"
    "libnvidia-gl-535:i386"
)
for pkg in "${packages[@]}"
do
    sudo apt-mark hold "$pkg"
done
```

#### 锁死linux内核

```
sudo apt-mark hold linux-image-generic linux-headers-generic 
```

## 依然可能存在的问题

重启服务器后可能会出现

```
nvidia-smi
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.
```

实际nvidia-smi已经是对应版本了，但是依然无法使用，此时通过再次运行官方安装的run文件来重新安装驱动解决，也可以自行加载NVIDIA驱动模块到内核解决。

所以那个下好的run文件还是保留着吧，以防万一，千万别删了。
