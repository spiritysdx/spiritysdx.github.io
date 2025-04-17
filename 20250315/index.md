# 通过dockur制作可迁移的Windows镜像(双文件)


## 使用dockur的原始镜像制作Windows镜像(双文件)

https://github.com/dockur/windows

原始仓库的镜像默认只是一个ISO下载器和网络自动设置器，本质上不包含Windows镜像，容器启动后镜像默认会将系统安装到挂载出的盘中。

本页说明最终将保存两个文件，一个是挂载盘的压缩文件，一个是容器导出的tar包。

仓库说明写的Windows镜像大小是ISO的大小，不是实际安装后整个镜像的大小，实际镜像大小大约是说明的三倍。

### 1. 拉取镜像

```shell
docker pull dockurr/windows
```

### 2. 设置需要的基础镜像

#### 通过官方源下载安装（非常缓慢）

```shell
export WINV="11"
```

开设镜像：

```shell
docker run -it -d -e RAM_SIZE="8G" -e CPU_CORES="4" --name win11 -p 8007:8006 --env VERSION="${WINV}" --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN -v /data/windows:/storage --stop-timeout 120 dockurr/windows
```

整个开设过程非常缓慢。

#### 使用自定义的ISO镜像安装（速度很快）

下载镜像大概10分钟：

```shell
wget https://down.idc.wiki/ISOS/Windows/Server%202022/zh-cn_windows_server_2022_x64_dvd_6c73507d.iso && chmod 777 zh-cn_windows_server_2022_x64_dvd_6c73507d.iso
```

开设镜像：

```shell
docker run -it -d -e RAM_SIZE="8G" -e CPU_CORES="4" --name win2022 -p 8007:8006 --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN -v /data/windows:/storage -v ./zh-cn_windows_server_2022_x64_dvd_6c73507d.iso:/boot.iso --stop-timeout 120 dockurr/windows
```

整个开设过程大约需要90分钟。

### 3. 导出镜像及使用

提交镜像：

```shell
docker commit windows11 spiritlhl/wds:11
```

导出docker镜像：

```shell
docker save -o wds_11.tar spiritlhl/wds:11
```

这里挂载出来的`/data/windows`需要被压缩保存。`pigz`需要提前下载，使用`-9`压缩率最大，压缩过程大约需要10多分钟：

```shell
tar -cvf - /data/windows | pigz -9 > archive.tar.gz
```

### 4. 迁移与恢复

如果迁移后开设新的容器，迁移时需要同时带上docker层面导出的tar包和这个文件夹的压缩文件，放到新文件夹中，开设时使用相同方式进行挂载。

加载环境并解压到当前目录，解压过程大约需要10多分钟：

```shell
pigz -d -c archive.tar.gz | tar -xvf -
```

解压后会在当前目录下，若需要更改路径，可使用`mv`命令修改实际路径。

加载镜像并再次开设（假设环境压缩包解压后仍为原路径，如果非原路径，则将`/data/windows`替换为实际路径）：

```shell
docker load -i wds_11.tar && docker run -it -d --name windows -p 8007:8006 --env VERSION="${WINV}" --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN -v /data/windows:/storage --stop-timeout 120 spiritlhl/wds:11
```

这样就能实现网络重新设置和环境加载，加载过程在10分钟以内。

### 5. 安装过程中自动执行的第三方脚本

通过挂载一个带`install.bat`文件的文件夹到容器的`/oem`路径上，容器安装Windows的过程中将自动执行本文件。

## 缺点

这样打包需要存储两个文件，一个文件16G一个文件500多MB，每次导入的时候都得加载两个文件不是很方便，未来整个魔改的方法只保存一个文件。

在浏览器端的VNC无法使用复制粘贴功能，这块没有解决的方法，系QEMU本身不支持的问题(仅限Windows虚拟机)
