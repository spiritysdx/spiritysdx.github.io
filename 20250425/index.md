# 通用docker容器镜像打包应用的流程


## 基础镜像选择

- `scratch` - 空白模板
- `debian:bookworm-slim` - debian的基础模板
- `ubuntu:22.04` - ubuntu的官方模板
- [linuxserver的KASM模板](https://github.com/linuxserver/docker-baseimage-kasmvnc?tab=readme-ov-file#available-distros) - `ghcr.io/linuxserver/baseimage-kasmvnc:debianbookworm`
- [kasmtech的第三方应用程序KASM模板](https://github.com/kasmtech/workspaces-images)
- [jlesage的NOVNC模板](https://github.com/jlesage/docker-baseimage-gui?tab=readme-ov-file#images) - `jlesage/baseimage-gui:ubuntu-22.04-v4`

由于镜像需要安装显卡驱动，所以这里选择了Ubuntu基础的Novnc的模板。如果不需要安装显卡驱动，换debian基础的也可以，镜像体积会更小一些。

## 如何选择基础镜像模板

- **空白模板**：只适合不依赖任何环境的二进制文件执行，或复制其他容器依赖文件的时候，一般来说不追求极致的精简大小用不上。
- **alpine镜像**：不追求极致的精简大小也用不上，alpine安装依赖如果特别多的时候比较难适配。
- **debian基础模板**：适合有一定的依赖需求，但又需要尽量减少容器镜像大小的时候，比如装 vscode windows 的时候。
- **ubuntu模板**：适合强依赖，比如装 Jupyter 的时候，需要安装大量依赖，还有就是安装Nvidia的驱动的时候，别的系统可能很难找到驱动。

安装依赖的困难程度：ubuntu < debian < alpine < scratch

- **KASM模板和NOVNC模板**：适合应用本身不支持浏览器在线浏览，需要起一个图形界面的环境的时候，比如在制作 eclipse、idea、pycharm、webstorm 的时候。
  - KASM模板支持传输声音，受控端如果有传输声音的需求可以使用它，但KASM不支持非HTTPS下的复制粘贴功能。
  - NOVNC模板不支持传输声音，但支持HTTP/HTTPS下的复制粘贴功能，一般来说用这个比较多。

具体看需求和时间成本选择基础镜像。

## 镜像是否需要更换软件源

一般来说，在中国境内打包应用，软件源一般是需要更换的，非中国镜像源在执行包管理器更新的时候非常缓慢。

这里可以使用 [https://linuxmirrors.cn/](https://linuxmirrors.cn/) 进行自动换源，例子：

```shell
RUN curl -lk https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh -o ChangeMirrors.sh && chmod 777 ChangeMirrors.sh \
    ./ChangeMirrors.sh --source mirrors.tuna.tsinghua.edu.cn --web-protocol http --intranet false --close-firewall true --backup true --updata-software false --clean-cache false --ignore-backup-tips
```

也可自行搜索对应软件源进行替换，常见镜像源：阿里云、腾讯云、清华

清华换源例子：

```shell
echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian bookworm main contrib non-free non-free-firmware\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian bookworm-updates main contrib non-free non-free-firmware\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware" \
> /etc/apt/sources.list
```

如果镜像本身默认携带的软件源速度还可以，不加入换源这一步也行。

## 设置支持中文

如果基础的镜像本身不支持中文显示，那么以ubuntu基础镜像为例子，下面是添加中文支持的环境安装和设置：

```shell
# 添加中文语言和字体支持
RUN apt-get update && apt-get install -y \
    language-pack-zh-hans \
    fonts-noto-cjk \
    fonts-wqy-microhei \
    && locale-gen zh_CN.UTF-8 && \
    update-locale LANG=zh_CN.UTF-8
ENV LANG=zh_CN.UTF-8
ENV LANGUAGE=zh_CN:zh
ENV LC_ALL=zh_CN.UTF-8
```

## 安装依赖

例子：

```shell
RUN apt-get update && apt-get install -y \
    python3-pip \
    openjdk-17-jdk \
    libxrender1 \
    libxtst6 \
    libxi6 \
    libxext6 \
    libfontconfig1 \
    libfreetype6 \
    libx11-6 \
    git \
    vim \
    nano \
    sudo \
    build-essential \
    dkms \
    curl \
    initramfs-tools
```

`apt-get update` 更新软件源

然后就是安装依赖：常见依赖有 git、nano、vim、sudo、wget、curl 一般都用得上，最好默认安装上。

其他依赖就看是否需要安装驱动或者别的特殊环境，比如：

- 要装 python3-pip 是因为容器内要安装pip依赖，例子：

```shell
# 安装ray
RUN pip install ray==2.9.0 --index-url https://pypi.tuna.tsinghua.edu.cn/simple
RUN pip install ray[client]==2.9.0 --index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

- 要装 build-essential、initramfs-tools、dkms 是因为容器内要安装显卡驱动
- 要装 openjdk-17-jdk 是因为容器内要使用java环境，同时需要设置：

```shell
# 设置 JAVA_HOME
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV PATH=$JAVA_HOME/bin:$PATH
```

## 安装pytorch

前置条件是容器内已安装了 python3-pip 包

同时这里需要查找对应显卡驱动的pytorch版本安装，具体命令参考下面这个示例：

```shell
# 安装对应版本的pytorch 见 https://pytorch.org/get-started/previous-versions/
RUN pip install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 -f https://pytorch.org/get-started/previous-versions/#install-pytorch-using-pip-cu124 --index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

## 编译的默认用户选择

Dockerfile编译的时候很多基础镜像设置的默认用户并不是`root`用户，导致编译出的镜像默认用户在一些路径上缺少操作权限。

这里推荐在编译过程中，在真正安装和启动应用之前，设置一下使用`root`用户，提高权限避免上述问题：

```shell
# 确保运行程序的用户非 app 用户
ENV USER_ID=0
ENV GROUP_ID=0
# 修复 /etc/passwd 中的 app 用户问题
RUN sed -i '/app::/d' /etc/passwd
```

## 编译的默认路径设置

编译时若不指定默认路径，那么容器默认的bash命令的路径就可能会随着基础镜像的用户变更为不可操作的路径。

这里推荐在编译过程中，在真正安装和启动应用之前，设置一下具体的容器默认路径，避免应用在启动过程中，一些设置文件被写入到不能持久化保存/无操作权限的路径中：

```shell
# 设置工作目录
WORKDIR /home/ubuntu/
# 设置默认 home 目录
ENV HOME=/home/ubuntu/
```

## 安装驱动

### Nvidia

需要先确保宿主机本身已经安装了显卡驱动，否则容器内安装了驱动也无法与显卡通信：

```shell
nvidia-smi
```

宿主机上执行后可见：

```
root@a12-ThinkStation-P620:~/lxd# nvidia-smi
Mon May 13 23:42:04 2024       
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.171.04             Driver Version: 535.171.04   CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA RTX A6000               Off | 00000000:61:00.0 Off |                  Off |
| 30%   27C    P8              11W / 300W |    168MiB / 49140MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+
                                                                                         
+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|    0   N/A  N/A      1713      G   /usr/lib/xorg/Xorg                           75MiB |
|    0   N/A  N/A      2108      G   /usr/bin/gnome-shell                         61MiB |
+---------------------------------------------------------------------------------------+
```

这里的宿主机安装的驱动版本为 535.171.04

那么容器内最好也安装这个版本，如果对应系统找不到这个版本，那么就找比这个版本低一点的版本安装，驱动一般是向下兼容的，不向上兼容。

容器内安装驱动，需要在 [https://www.nvidia.cn/Download/Find.aspx?lang=cn](https://www.nvidia.cn/Download/Find.aspx?lang=cn) 中查询对应显卡版本对应基础镜像的系统的驱动。

下载到宿主机上后，Dockerfile中可以用COPY命令COPY进去使用，如：

```shell
COPY NVIDIA-Linux-x86_64-550.135.run /tmp/NVIDIA-Linux-x86_64-550.135.run
```

容器内有了这个驱动后，且默认的依赖安装了：

- build-essential
- dkms
- initramfs-tools

那么后续就可以：

```shell
# 禁用 Nouveau 驱动
RUN echo -e "blacklist nouveau\noptions nouveau modeset=0" > /etc/modprobe.d/blacklist-nouveau.conf && \
    update-initramfs -u
# 安装 NVIDIA 550.135 驱动（自动化、静默安装）
RUN chmod +x /tmp/NVIDIA-Linux-x86_64-550.135.run && \
    /tmp/NVIDIA-Linux-x86_64-550.135.run --no-kernel-module --silent --accept-license
```

这样安装的驱动注意是一次性的，如果容器内的驱动在容器意外重启过程中被别的进程损坏，那么这里最好保留这个run文件到容器的`/home`或`/root`路径下，驱动出问题了用户可以自行用该文件修复。

### Ascend

需要先确保宿主机本身已经安装了昇腾卡的显卡驱动，否则容器内安装了驱动也无法与显卡通信：

```shell
npu-smi info
```

宿主机上执行后可见：

```
[root@master-118 ~]# npu-smi info
+--------------------------------------------------------------------------------------------------------+
| npu-smi 24.1.0.1                                 Version: 24.1.0.1                                     |
+-------------------------------+-----------------+------------------------------------------------------+
| NPU     Name                  | Health          | Power(W)     Temp(C)           Hugepages-Usage(page) |
| Chip    Device                | Bus-Id          | AICore(%)    Memory-Usage(MB)                        |
+===============================+=================+======================================================+
| 5       310P3                 | OK              | NA           59                0     / 0             |
| 0       0                     | 0000:81:00.0    | 0            1492 / 44280                            |
+-------------------------------+-----------------+------------------------------------------------------+
| 5       310P3                 | OK              | NA           58                0     / 0             |
| 1       1                     | 0000:81:00.0    | 0            1426 / 43693                            |
+===============================+=================+======================================================+
+-------------------------------+-----------------+------------------------------------------------------+
| NPU     Chip                  | Process id      | Process name             | Process memory(MB)        |
+===============================+=================+======================================================+
| No running processes found in NPU 5                                                                    |
+===============================+=================+======================================================+
```

那么容器内可以直接COPY宿主机的环境做昇腾卡的驱动部署：

```shell
ENV LD_LIBRARY_PATH=/usr/local/Ascend/driver/lib64/common:$LD_LIBRARY_PATH
ENV LD_LIBRARY_PATH=/usr/local/Ascend/driver/lib64/driver:$LD_LIBRARY_PATH
COPY dcmi /usr/local/dcmi
COPY npu-smi /usr/local/bin/npu-smi
COPY common /usr/local/Ascend/driver/lib64/common
COPY driver /usr/local/Ascend/driver/lib64/driver
COPY ascend_install.info /etc/ascend_install.info
COPY vnpu.cfg /etc/vnpu.cfg
COPY version.info /usr/local/Ascend/driver/version.info
RUN chmod +x /usr/local/bin/npu-smi
```

这样就安装进去了。

### 容器内驱动测试

英伟达显卡和昇腾显卡的docker启动命令挂载显卡是不同的：

英伟达显卡的docker启动命令中加入：

```shell
--gpus device=0
```

昇腾显卡的docker启动命令中加入：

```shell
-e ASCEND_VISIBLE_DEVICES=0 --device=/dev/davinci0 --device=/dev/davinci_manager --device=/dev/devmm_svm --device=/dev/hisi_hdc -v /usr/local/Ascend:/usr/local/Ascend -v /usr/local/dcmi:/usr/local/dcmi 
```

然后：

```shell
docker exec -it 容器名字 /bin/bash
```

进入容器后执行：

```shell
nvidia-smi
```

或：

```shell
npu-smi info
```

检测容器内的显卡驱动是否可用。

## 安装应用

### 需要图形界面环境(以jetbrains系列的应用为例)

需要先下载linux的安装包：[https://www.jetbrains.com.cn/ides/](https://www.jetbrains.com.cn/ides/)

注意有的在线IDE是不存在community社区版本的，此时只能下载开发者版本然后试用，简单的说有社区版本就下社区版本。

然后将社区版本的linux的tar.gz包解压，因为需要在Dockerfile中指定其config的路径，所以需要：

```shell
tar zxvf 文件名.tar.gz
```

在当前目录解压这个tar.gz包，解压后可见到类似 `pycharm-community-2024.2.1` 这样的文件夹名字，那么这个文件夹就是后续要用到的名字，然后这个文件夹就可以删除了，后续用不上，只是为了记下名字用的。

Dockerfile中需要如下设置应用，这块的文件夹名字使用你实际解压得到的名字：

```shell
# 拷贝 PyCharm 安装包
COPY pycharm-community-2024.2.1.tar.gz /tmp/pycharm.tar.gz
# 解压 PyCharm 并创建快捷方式
RUN tar -xzf /tmp/pycharm.tar.gz -C /opt && \
    ln -s /opt/pycharm-community-2024.2.1/bin/pycharm.sh /usr/local/bin/pycharm && \
    rm /tmp/pycharm.tar.gz
# 创建启动脚本
RUN echo '#!/bin/sh' > /startapp.sh && \
    echo 'exec pycharm' >> /startapp.sh && \
    chmod +x /startapp.sh
# 配置版本信息
RUN echo "APP_NAME=PyCharm Community" >> /etc/environment && \
    echo "APP_VERSION=2024.2.1" >> /etc/environment
# 修改默认的 PyCharm 配置，持久化设置
RUN tee -a /opt/pycharm-community-2024.2.1/bin/idea.properties <<EOF
idea.system.path=/home/ubuntu/system
idea.config.path=/home/ubuntu/config
idea.plugins.path=/home/ubuntu/plugins
idea.default.project.directory=/home/ubuntu/
idea.no.launcher.banner=true
idea.disable.protocol.confirmation=true
EOF
```

这块在配置的时候，注意`/home/ubuntu`必须是原基础镜像中存在的(`jlesage/baseimage-gui:ubuntu-22.04-v4`中肯定存在)，如果不存在的话需要先：

```shell
RUN mkdir -p 持久化的home中的文件夹名字
```

创建路径，自行替换配置的文件夹名字。

这块的启动脚本是配合`jlesage/baseimage-gui:ubuntu-22.04-v4`进行使用的。

如果使用的是KASM模板配置，那么使用的需要查找对应的仓库的说明。

需要图形界面环境的应用的启动命令不需要自行设计，必须交由原基础镜像的启动命令操作，不要修改它，否则图形界面环境启动不了。

### 不需要图形界面环境(以自带可浏览器在线浏览的应用为例)

比如 Jupyter 比如 code-server(vscode)

这种应用就不需要安装NOVNC/KASM进行桌面环境的初始化了，直接可以使用基础的linux镜像进行制作。

jupyter可以使用miniconda进行安装，参照 [https://github.com/spiritLHLS/one-click-installation-script/blob/main/install_scripts/jupyter.sh](https://github.com/spiritLHLS/one-click-installation-script/blob/main/install_scripts/jupyter.sh)

可写：

```shell
# 安装 Miniconda
RUN wget -O /tmp/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    bash /tmp/miniconda.sh -b -p /opt/conda && \
    rm /tmp/miniconda.sh
```

这块安装注意架构是amd64还是arm64，amd64写的是x86_64的安装脚本，arm64写的是aarch64的安装脚本。

```shell
# 初始化 Conda 并创建 Jupyter 环境
RUN /opt/conda/bin/conda init bash && \
    echo "source /opt/conda/bin/activate" >> ~/.bashrc && \
    conda config --set auto_activate_base false && \
    conda create -n jupyter_env python=3.10 -y && \
    conda clean -afy
```

这块在定义具体需要指定安装什么版本的python。

```shell
# 安装 Jupyter 及插件
RUN /opt/conda/bin/conda run -n jupyter_env pip install --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple --timeout=300 \
    jupyterlab notebook jupyter_server jupyter_http_over_ws jupyter-ai[all] && \
    /opt/conda/bin/conda run -n jupyter_env jupyter server extension enable --py jupyter_http_over_ws
```

这块在定义需要安装所有jupyter的组件，并启用jupyter的插件。

```shell
# 写入 Jupyter 配置文件
RUN mkdir -p /root/.jupyter && \
    cat <<EOF > /root/.jupyter/jupyter_server_config.py
c.ServerApp.allow_root = True
c.ServerApp.open_browser = False
c.ServerApp.password = ''
c.ServerApp.ip = '0.0.0.0'
c.ServerApp.token = ''
c.ServerApp.tornado_settings={ \"headers\": { \"Content-Security-Policy\": \"frame-ancestors 'self' *\"} }
c.ServerApp.disable_check_xsrf = True
c.ServerApp.allow_origin = '*'
c.ServerApp.port = 31000
c.ServerApp.allow_remote_access = True
EOF
```

这块在定义jupyter的默认配置。

```shell
CMD ["bash", "-c", "\
base_url=$(echo \"$NOTEBOOK_ARGS\" | sed -n \"s/.*--ServerApp.base_url='\\([^']*\\)'.*/\\1/p\") && \
conda init bash && source ~/.bashrc && conda activate jupyter_env && \
if [ -n \"$base_url\" ]; then \
  jupyter lab --allow-root --port=31000 --ip=0.0.0.0 --NotebookApp.token='' --NotebookApp.password='' --ServerApp.base_url=\"$base_url\"; \
else \
  jupyter lab --allow-root --port=31000 --ip=0.0.0.0 --NotebookApp.token='' --NotebookApp.password=''; \
fi"]
```

启动命令由于需要外传指定jupyter的启动路径，所以写成这个非常复杂。

不需要图形界面环境的应用的启动命令需要自行设计，不能交由原基础镜像的启动命令操作。

## 容器暴露端口

针对`jlesage/baseimage-gui:ubuntu-22.04-v4`基础镜像：

```shell
# 配置 VNC 服务器参数
ENV DISPLAY=:0
# 允许自动登录
ENV GUI_AUTOLOGIN=1
# 使用 root 用户启动 Eclipse
ENV GUI_USER=root
# VNC 端口（用于 VNC 客户端）
ENV VNC_PORT=5900
# noVNC 端口（用于 Web 浏览器访问）
ENV WEB_LISTENING_PORT=31000
# 启用剪贴板共享
ENV ENABLE_CLIPBOARD=1
# 暴露端口
EXPOSE 5900 31000
```

针对`debian:bookworm-slim`等仅系统的基础镜像：

```shell
# 暴露端口
EXPOSE 5900 31000
```

## 设置容器支持ssh连接

### 如何测试容器基础镜像带什么环境

```shell
docker exec -it 容器名字 /bin/bash
```

这种能进去的就是带bash环境的，报错的话试试：

```shell
docker exec -it 容器名字 /bin/sh
```

这种进得去的就是带sh环境的。

最好使用带bash环境的基础镜像，带sh环境的基础镜像有一些shell命令的语法无法使用。

### 基础镜像是带bash环境的

如：Debian、Ubuntu、Centos、Almalinux、Redhat等

参考 [https://github.com/oneclickvirt/docker/blob/main/scripts/ssh_bash.sh](https://github.com/oneclickvirt/docker/blob/main/scripts/ssh_bash.sh)

```shell
# 依赖安装
RUN apt-get update && apt-get install -y openssh-server && \
    mkdir -p /var/run/sshd
# 修改 sshd_config
RUN config_file="/etc/ssh/sshd_config" && \
    sed -i "s/^#\?Port.*/Port 22/g" "$config_file" && \
    sed -i "s/^#\?PermitRootLogin.*/PermitRootLogin yes/g" "$config_file" && \
    sed -i "s/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g" "$config_file" && \
    sed -i 's/#ListenAddress 0.0.0.0/ListenAddress 0.0.0.0/' "$config_file" && \
    sed -i 's/#ListenAddress ::/ListenAddress ::/' "$config_file" && \
    sed -i 's/#AddressFamily any/AddressFamily any/' "$config_file" && \
    sed -i "s/^#\?PubkeyAuthentication.*/PubkeyAuthentication no/g" "$config_file" && \
    sed -i '/^#UsePAM\|UsePAM/c #UsePAM no' "$config_file" && \
    sed -i '/^AuthorizedKeysFile/s/^/#/' "$config_file" && \
    sed -i 's/^#[[:space:]]*KbdInteractiveAuthentication.*\|^KbdInteractiveAuthentication.*/KbdInteractiveAuthentication yes/' "$config_file"
# 设置 root 密码（这里设置为 password，按需更改）
RUN echo 'root:password' | chpasswd
# 启动SSH服务
RUN service sshd restart || true
RUN systemctl restart sshd || true
RUN systemctl restart ssh || true
RUN /usr/sbin/sshd || true
RUN service ssh restart || true
```

上面这里安装的`openssh-server`可以加入到之前的依赖安装中去，这里仅示例，不必另外写一次依赖安装。

### 基础镜像仅带sh环境的

如：Alpine

参考 [https://github.com/oneclickvirt/docker/blob/main/scripts/ssh_sh.sh](https://github.com/oneclickvirt/docker/blob/main/scripts/ssh_sh.sh)

## 容器内端口转发更改默认端口

有时候默认的程序的端口不符合需求，比如平台已经写死了转发容器内的`31000`端口做web端的端口，此时就需要修改原先的`8006`为`31000`端口

这里需要一个通用的转发容器内部端口的方法，但注意，这种方法不适合转发NoVNC的端口，因为涉及ws协议头，没法转换，这种方法只能转换简单的nginx的tcp协议，不涉及协议头加密升级等内容，涉及NoVNC的端口改变还是得改配置文件

此时需要使用`socat`进行端口转发

```shell
socat TCP-LISTEN:31000,fork,reuseaddr TCP:127.0.0.1:8006 &
```

所以需要如下操作：

在Dockerfile的同级目录下执行

```shell
touch entry-wrapper.sh
```

然后`nano`或者`vim`写入

```shell
#!/usr/bin/env bash
set -e

# 1. 启动端口转发（8006 → 31000）
#    fork 到后台，不影响后面的 exec
socat TCP-LISTEN:31000,fork,reuseaddr TCP:127.0.0.1:8006 &

# 2. 把日志重定向（可选）
# exec >> /var/log/entry-wrapper.log 2>&1

# 3. 最后 exec 回到原来的 entry.sh（并保持 tini 处理信号）
exec /usr/bin/tini -s /run/entry.sh "$@"
```

Dockerfile中修改`ENTRYPOINT ["/usr/bin/tini", "-s", "/run/entry.sh"]`改为

```shell
COPY --chmod=755 entry-wrapper.sh /usr/bin/entry-wrapper.sh
ENTRYPOINT ["/usr/bin/entry-wrapper.sh"]
```

修改默认的web的端口

```shell
# EXPOSE 8006
EXPOSE 31000
```

这样操作后，`socat`在前面监听`31000`，把它转发到`8006`，又不影响原先的所有nginx的配置

## 容器镜像固化

在对容器进行完毕所有要修改的操作之后，在打包之前，需要commit一下：

```shell
docker commit 容器名字 镜像名字
```

## 容器镜像导出

```shell
docker save -o 名字.tar 镜像名字
```

导出镜像文件的tar包的过程中，需要保证当前路径的挂载点还有非常多的空闲硬盘，否则会写入报错。

```shell
lsblk
```

```shell
df -h
```

查看确保空间足够再进行导出。

## 容器镜像导入

```shell
docker load -i 名字.tar
```

导入后就可以使用镜像了。

