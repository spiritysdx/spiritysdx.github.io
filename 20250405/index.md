# 魔改dockur制作可迁移的Windows镜像(单文件)


## 修改dockur制作Windows镜像

<https://github.com/dockur/windows>

拉取测试的版本: [v4.33](https://github.com/dockur/windows/tree/v4.33)

原始仓库的镜像默认只是一个ISO下载器和网络自动设置器，本质上不包含Windows镜像，容器启动后镜像会将系统安装到挂载出的盘中。

由于默认的镜像存在无法持久化保存修改的问题，这里需要修改镜像并重新打包。最终只会保存一个文件，即容器导出的tar包。

tar包打出来的大小会比较大，目前我打出来是26.3G。(这个大小不受系统内安装了多少软件影响，这是写入卷默认的大小)

### 1. 克隆仓库

```shell
git clone https://github.com/dockur/windows.git
```

```shell
cd windows
```

由于需要进入其`src`目录下的`install.sh`，先执行以下命令先查询行：

```shell
sed -n '/rm -f "\$STORAGE\/windows\.old"/,/rm -f "\$STORAGE\/windows\.type"/p' src/install.sh
```

查看结果是否为：

```
  rm -f "$STORAGE/windows.old"
  rm -f "$STORAGE/windows.vga"
  rm -f "$STORAGE/windows.net"
  rm -f "$STORAGE/windows.usb"
  rm -f "$STORAGE/windows.args"
  rm -f "$STORAGE/windows.base"
  rm -f "$STORAGE/windows.boot"
  rm -f "$STORAGE/windows.mode"
  rm -f "$STORAGE/windows.type"
```

如果是的话，执行以下命令删除这些行：

```shell
sed -i '/rm -f "\$STORAGE\/windows\.old"/,/rm -f "\$STORAGE\/windows\.type"/d' src/install.sh
```

### 2. 修改`Dockerfile`

```Dockerfile
ARG VERSION_ARG="latest"
# FROM scratch AS build-amd64
FROM debian:bookworm-slim AS build-amd64

COPY --from=qemux/qemu:7.11 / /

RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian bookworm main contrib non-free non-free-firmware\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian bookworm-updates main contrib non-free non-free-firmware\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware" \
> /etc/apt/sources.list

ARG DEBCONF_NOWARNINGS="yes"
ARG DEBIAN_FRONTEND="noninteractive"
ARG DEBCONF_NONINTERACTIVE_SEEN="true"

RUN set -eu && \
    apt-get update && \
    apt-get --no-install-recommends -y install \
        wsdd \
        samba \
        wimtools \
        dos2unix \
        cabextract \
        libxml2-utils \
        libarchive-tools \
        netcat-openbsd && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# COPY --chmod=755 ./src /run/
# COPY --chmod=755 ./assets /run/assets
# ADD --chmod=664 https://github.com/qemus/virtiso-whql/releases/download/v1.9.45-0/virtio-win-1.9.45.tar.xz /drivers.txz

COPY ./src /run/
RUN chmod -R 755 /run/
COPY ./assets /run/assets/
RUN chmod -R 755 /run/assets/
ADD https://cdn.spiritlhl.net/https://github.com/qemus/virtiso-whql/releases/download/v1.9.45-0/virtio-win-1.9.45.tar.xz /drivers.txz
RUN chmod 664 /drivers.txz

# FROM dockurr/windows-arm:${VERSION_ARG} AS build-arm64
# FROM build-${TARGETARCH}
# VOLUME /storage
RUN mkdir -p /storage && df -h /storage
ARG VERSION_ARG="0.00"
RUN echo "$VERSION_ARG" > /run/version

EXPOSE 3389 8006

ENV RAM_SIZE="4G"
ENV CPU_CORES="2"
# 减少C盘默认大小，实际使用最多16G
# ENV DISK_SIZE="64G"
ENV DISK_SIZE="20G"

ENTRYPOINT ["/usr/bin/tini", "-s", "/run/entry.sh"]
```

参考上面的`Dockerfile`进行修改：

- 加入`docker.spiritlhl.news`加速原始镜像下载。
- 注释掉`VOLUME /storage`，去除挂载并改成`RUN mkdir -p /storage && df -h /storage`。
- 将带`--chmod`的行改成使用`COPY`和`RUN chmod`的两行命令。
- 注释掉`FROM build-${TARGETARCH}`。

如果当前部署机是x86的，就注释掉以下内容：

```dockerfile
FROM dockurr/windows-arm:${VERSION_ARG} AS build-arm64
```

并修改`FROM scratch AS build-amd64`为：

```dockerfile
FROM debian:bookworm-slim AS build-amd64
```

同时加入清华源加速部分依赖下载，注意不能删除`/etc/apt/sources.list.d/`下的文件，有一些包必须通过官方源下载即便很慢：

```dockerfile
RUN echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian bookworm main contrib non-free non-free-firmware\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian bookworm-updates main contrib non-free non-free-firmware\n\
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware" \
> /etc/apt/sources.list
```

如果当前部署机是arm64的，就注释掉以下内容：

```dockerfile
FROM scratch AS build-amd64
```

### 3. 打包镜像并导出tar包

执行以下命令打包镜像并导出tar包：

```shell
docker build -t windows:builder .
docker save -o builder.tar windows:builder
```

### 4. 下载Windows镜像

在 [https://down.idc.wiki/ISOS/Windows/](https://down.idc.wiki/ISOS/Windows/) 中找到要使用的ISO镜像进行下载，例如：

```shell
wget https://down.idc.wiki/ISOS/Windows/Server%202022/zh-cn_windows_server_2022_x64_dvd_6c73507d.iso && chmod 777 zh-cn_windows_server_2022_x64_dvd_6c73507d.iso
```

下载镜像大概需要10分钟。

或者不使用这里的镜像，下面开设的时候不挂载```-v ./zh-cn_windows_server_2022_x64_dvd_6c73507d.iso:/boot.iso```而是使用```--env VERSION="2019"```这种方法指定下载官方的iso文件，也可以。这块参考 <https://github.com/dockur/windows> 仓库中的说明。

### 5. 开设虚拟机容器

使用下载的ISO镜像开设虚拟机容器：

```shell
docker load -i builder.tar
```

```shell
docker run -it -d -e RAM_SIZE="8G" -e CPU_CORES="4" --name win2022 -p 8006:8006 --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN -v ./zh-cn_windows_server_2022_x64_dvd_6c73507d.iso:/boot.iso --stop-timeout 120 windows:builder
```

整个开设过程大约需要90分钟。

### 6. 设置容器无需重新下载和安装镜像

执行

```shell
docker exec win2022 sh -c 'echo "data.img" > /storage/windows.boot'
```

PS: 本操作借鉴自 <https://github.com/dockur/windows/issues/1192>

### 7. 设置虚拟机C盘自动扩容

由于原先系统制作的时候Dockerfile中预定义的硬盘为20G，实际开设过程中需要增大硬盘大小，否则会出现C盘使用一段时间后磁盘写满的情况，所以需要在开设的时候指定扩容硬盘，但这样依然没有自动扩容硬盘到C盘，虚拟机中进入磁盘管理器后可以看到有未分配的硬盘。

需要在打包镜像前创建一个bat脚本，创建定时任务在系统启动后执行

```bat
@echo off
net session >nul 2>&1
if %errorlevel% neq 0 (
    exit /b
)
set "dptemp=%temp%\extend_c_drive.txt"
> "%dptemp%" (
    echo select volume C
    echo extend
)
diskpart /s "%dptemp%"
del "%dptemp%"
```

在 设置 -- 控制面板 -- 计划任务 -- 创建基本任务 -- 当前用户登录时启动任务(L) -- 选择这个bat程序文件 -- 创建任务

### 8. 设置开机自启程序

类似上述操作，事先查找到要开机自启动的桌面的程序路径，右键点开图标的```属性(R)```，复制粘贴```属性(R)```中的```目标(T)```的地址，替换下面的```"路径"```为对应的路径

```bat
@echo off
setlocal enabledelayedexpansion
start "" "路径"
```

### 9. 提交并导出最终镜像

提交虚拟机容器并导出镜像：

```shell
docker commit win2022 windows:2022 && docker save -o win2022.tar windows:2022
```

注意当前宿主机必须至少拥有100G空闲的硬盘在路径```/```上，否则会出现类似```write ./.docker_temp_336929345: no space left on device```这样的报错。

这个导出并保存为文件的命令执行时间在60分钟左右。

### 10. 加载并运行最终镜像

如果使用这个tar包开设虚拟机，执行以下命令：

```shell
docker load -i win2022.tar && docker run -it -d -e RAM_SIZE="8G" -e CPU_CORES="4" --name win2022 -p 8006:8006 --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN --stop-timeout 120 windows:2022
```

这个过程大概需要5分钟。

如果需要增加硬盘大小，那么就设置

```shell
-e DISK_SIZE="35G"
```

如果需要外挂数据，那么就

```shell
-v 宿主机的那个文件夹的绝对路径:/data
```

然后在开设的win中：

- 打开`文件资源管理器`。
- 点击左侧的`网络`选项。
- 找到名为`host.lan`的计算机。
- 双击`host.lan`，看到一个名为`Data`的共享文件夹，该共享文件夹对应挂载的文件夹。

### 关于GPU

这样制作的镜像GPU无法在容器内给到Windows的虚拟机使用的，即便可以在容器中安装驱动并成功使用，也无法转发给容器内的虚拟机

相关尝试 <https://github.com/dockur/windows/issues/22>

### 关于端口

只能使用官方给的端口，无法自行修改，要修改的话需要修改基础镜像中复制的 qemus/qemu 镜像

目前容器内的端口是

web端启动的NoVNC端口是 8006

RDP启动的端口是 3389

如果需要使用对应服务，需要在docker启动时指定映射容器的这两个端口

## 后记

虽然这样持久化保存了系统镜像到了docker的写入层，但是镜像体积会非常大，我打包win的server的2022打包的tar包总计26.3G大小。

造成这样的原因是默认的挂载点被我修改删掉了，没法约束对应storage文件夹的大小了，默认就是docker分配的匿名挂载的大小。

同时启动速度极慢，不如直接通过ProxmoxVE或者Kubevirt的直接启动qcow2文件来得快。

在浏览器端的VNC无法使用复制粘贴功能，这块没有解决的方法，系QEMU本身不支持的问题(仅限Windows虚拟机)

