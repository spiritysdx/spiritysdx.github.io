# Red Hat 8 amd64 离线依赖适配记录


这篇记录整理 Red Hat 8 amd64 环境下为 BKE 部署准备离线依赖包的流程。核心思路是先在可联网环境中补齐 RPM 及其依赖，再把离线包复制到纯净测试机中验证安装，最后进入 allinone 部署。

## 适配环境

- 系统：Red Hat 8 amd64
- 镜像：<https://archive.org/download/rhel-8.1-x86_64-dvd/rhel-8.1-x86_64-dvd.iso>
- 目标：补齐 `docker-ce`、`iptables`、`lxcfs`、`nfs-utils` 等 BKE 运行依赖

## 系统换源

Red Hat 8 默认软件源在内网或离线场景下不可直接使用，可以先用 `linuxmirrors.cn` 切换到可用源：

```shell
bash <(curl -sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh)
```

实际测试中，换源后主要使用 CentOS 8 兼容源补包。

## 离线包下载

在联网环境中使用 `yumdownloader` 下载目标包：

```shell
yumdownloader 对应包名
```

依赖清单需要对照已有环境，例如 `server4` 上的：

```text
/data/bke/source_registry/CentOS/8/amd64
```

下载时注意架构问题：部分依赖不只有 `x86_64`，还会需要 `i686`。如果只下载 `x86_64` 包，离线安装阶段可能继续报缺依赖。

## 离线安装

安装前建议先处理系统自带的高版本 `iptables`。如果系统内置的是 nft 版本，后续可能影响容器网络规则：

```shell
sudo dnf remove iptables
```

将 RPM 包通过 `scp` 传输到测试机的空目录后，在该目录执行：

```shell
sudo dnf install ./*.rpm --allowerasing --nobest
```

需要重点确认以下组件能被正常安装和调用：

```text
docker-ce
iptables
lxcfs
nfs-utils
```

如果安装过程中继续提示缺少依赖，就回到联网环境下载对应依赖，再重新传回测试机重复安装验证。

## 部署验证

离线依赖提前安装完成后，不需要再让 BKE 重置系统环境。后续按正常 allinone 部署流程验证即可。

排障时优先确认三类问题：

- RPM 是否缺依赖或存在版本冲突。
- `iptables` 是否仍然是 nft 版本导致规则不兼容。
- `docker-ce`、`lxcfs`、`nfs-utils` 是否能在纯净环境中离线安装。

