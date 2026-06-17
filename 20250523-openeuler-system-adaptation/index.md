# openEuler 22.03 arm64 离线包适配记录


这篇记录 openEuler 22.03 LTS SP4 arm64 环境下制作离线依赖包的过程。目标是对齐已有 amd64 离线源，补齐 arm64 目录中缺失的 Docker、NFS 和基础工具包，并在纯净环境中验证安装冲突。

## 下载目录结构

按目标架构创建目录：

```shell
mkdir -p aarch64/{docker-ce,nfs-utils,tools}
```

离线包最终需要能放入类似目录：

```text
/data/bke/source_registry/OpenEuler/22/arm64
```

并与已有 amd64 目录做包名对齐。

## 批量下载脚本

以下脚本只使用 openEuler 源下载基础依赖，并尝试解析已有包的依赖：

```shell
#!/bin/bash
set -e

RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

log_info() { echo -e "${GREEN}[INFO]${NC} $1"; }
log_warn() { echo -e "${YELLOW}[WARN]${NC} $1"; }
log_error() { echo -e "${RED}[ERROR]${NC} $1"; }

check_root() {
    if [[ $EUID -ne 0 ]]; then
        log_error "需要root权限运行"
        exit 1
    fi
}

check_arch() {
    local arch
    arch=$(uname -m)
    if [[ "$arch" != "aarch64" ]]; then
        log_warn "当前系统架构为 $arch"
    fi
}

create_directories() {
    mkdir -p aarch64/{docker-ce,nfs-utils,tools}
}

download_packages() {
    local dir=$1
    shift
    local packages=("$@")
    cd "aarch64/$dir/"
    for package in "${packages[@]}"; do
        log_info "正在下载: $package"
        if yum download --downloadonly --downloaddir=. "$package" 2>/dev/null; then
            log_info "✓ $package 下载成功"
        else
            log_warn "✗ $package 下载失败 - 可能在 openEuler 源中不存在"
        fi
    done
    cd ../..
}

download_docker_packages() {
    local docker_packages=(
        "docker" "docker-engine" "containerd" "runc"
        "container-selinux" "fuse3" "fuse-overlayfs"
        "libcgroup" "slirp4netns" "tar" "socat" "checkpolicy"
    )
    download_packages "docker-ce" "${docker_packages[@]}"
}

download_nfs_packages() {
    local nfs_packages=(
        "gssproxy" "keyutils" "krb5-libs" "nfs-utils"
        "quota" "rpcbind" "libtirpc" "libnfsidmap"
    )
    download_packages "nfs-utils" "${nfs_packages[@]}"
}

download_tools_packages() {
    local tools_packages=(
        "haproxy" "jq" "keepalived" "lm_sensors"
        "net-snmp" "net-snmp-libs" "telnet" "nano"
        "curl" "wget" "vim" "htop"
    )
    download_packages "tools" "${tools_packages[@]}"
}

download_dependencies() {
    log_info "开始解析和下载依赖包..."
    for dir in aarch64/*/; do
        if [[ ! -d "$dir" ]]; then continue; fi
        cd "$dir" || continue
        local rpm_files
        mapfile -t rpm_files < <(find . -maxdepth 1 -name "*.rpm")
        if [[ ${#rpm_files[@]} -eq 0 ]]; then
            cd - >/dev/null
            continue
        fi
        log_info "为 $(basename "$dir") 解析依赖..."
        for rpm in "${rpm_files[@]}"; do
            local pkg_name
            pkg_name=$(rpm -qp --queryformat "%{NAME}" "$rpm" 2>/dev/null || true)
            if [[ -n "$pkg_name" ]]; then
                yum deplist "$pkg_name" 2>/dev/null | grep "provider:" | awk '{print $2}' | sort -u | while read -r dep; do
                    if [[ -n "$dep" && "$dep" != "Not" ]]; then
                        yum download --downloadonly --downloaddir=. "$dep" 2>/dev/null || true
                    fi
                done
            fi
        done
        cd - >/dev/null
    done
}

show_results() {
    log_info "下载结果统计:"
    local total=0
    for dir in docker-ce nfs-utils tools; do
        if [[ -d "aarch64/$dir" ]]; then
            local count
            count=$(find "aarch64/$dir" -name "*.rpm" 2>/dev/null | wc -l)
            echo "  $dir: $count 个包"
            total=$((total + count))
        fi
    done
    echo "  总计: $total 个包"
}

restore_repos() {
    log_info "恢复原有软件源配置..."
    rm -f /etc/yum.repos.d/openeuler-only.repo
    find /etc/yum.repos.d/ -name "*.repo.disabled" -exec bash -c 'mv "$1" "${1%.disabled}"' _ {} \;
}

cleanup() {
    restore_repos
    yum clean all >/dev/null 2>&1 || true
}

main() {
    log_info "开始架构离线包下载，仅使用 openEuler 源"
    check_root
    check_arch
    create_directories
    download_docker_packages
    download_nfs_packages
    download_tools_packages
    download_dependencies
    show_results
    cleanup
    log_info "任务完成，包已保存在 aarch64/ 目录下"
}

trap cleanup EXIT
main "$@"
```

## 初次安装验证

进入每个离线包目录执行：

```shell
dnf install -y ./*.rpm --nobest --allowerasing --skip-broken
```

如果有冲突，先删除明显重复或不应替换系统基础组件的包。例如：

```shell
cd docker-ce
rm -rf docker-engine-18.09.0-345.oe2203sp4.aarch64.rpm
rm -rf runc-1.1.3-30.oe2203sp4.aarch64.rpm
rm -rf containerd-1.2.0-321.oe2203sp4.aarch64.rpm
rm -rf systemd-*
cd ..

cd nfs-utils
rm -rf kernel-rt-headers-5.10.0-209.0.0.rt62.62.oe2203sp4.aarch64.rpm
rm -rf systemd-*
cd ..

cd tools
rm -rf libcurl*
rm -rf vim*
rm -rf curl*
rm -rf systemd-*
cd ..
```

这些包不是永远都要删除，原则是避免用离线包覆盖纯净系统内已经存在且版本受系统管理的基础组件。

## 单独补 Docker CE

openEuler 源里的 `docker-engine` 版本较老。Docker CE 需要对照 CentOS 8 aarch64 的包下载：

```shell
cd docker-ce
wget https://mirrors.aliyun.com/docker-ce/linux/centos/8/aarch64/stable/Packages/docker-ce-26.1.0-1.el8.aarch64.rpm
wget https://mirrors.aliyun.com/docker-ce/linux/centos/8/aarch64/stable/Packages/docker-ce-cli-26.1.0-1.el8.aarch64.rpm
wget https://mirrors.aliyun.com/docker-ce/linux/centos/8/aarch64/stable/Packages/containerd.io-1.6.31-3.1.el8.aarch64.rpm
wget https://mirrors.aliyun.com/docker-ce/linux/centos/8/aarch64/stable/Packages/docker-compose-plugin-2.26.1-1.el8.aarch64.rpm
wget https://mirrors.aliyun.com/docker-ce/linux/centos/8/aarch64/stable/Packages/docker-buildx-plugin-0.14.0-1.el8.aarch64.rpm
wget https://mirrors.aliyun.com/docker-ce/linux/centos/8/aarch64/stable/Packages/docker-ce-rootless-extras-26.1.0-1.el8.aarch64.rpm
```

参考目录：

<https://download.docker.com/linux/centos/8/aarch64/stable/Packages/>

## 对齐 amd64 与 arm64 包名

可以用下面脚本比较两个架构目录的 RPM 主包名：

```shell
#!/bin/bash
base_dir="/data/bke/source_registry/OpenEuler/22"
amd64_dir="$base_dir/amd64"
arm64_dir="$base_dir/arm64"

extract_names() {
    find "$1" -type f -name "*.rpm" | while read -r file; do
        rpm_file=$(basename "$file")
        echo "$rpm_file" | sed -E 's/-(.*)\.rpm$//'
    done | sort -u
}

amd64_pkgs=$(extract_names "$amd64_dir")
arm64_pkgs=$(extract_names "$arm64_dir")

echo "==== 在 arm64 中缺失的 amd64 包 ===="
comm -23 <(echo "$amd64_pkgs") <(echo "$arm64_pkgs")

echo "==== 在 amd64 中缺失的 arm64 包 ===="
comm -13 <(echo "$amd64_pkgs") <(echo "$arm64_pkgs")
```

这个比较结果不是直接照单全收，需要结合纯净环境安装结果判断哪些依赖是真缺失，哪些只是高版本或 `-devel` 包造成冲突。

## 纯净环境最终验证

重装系统为纯净环境后，先不要联网补依赖，直接测试离线包：

```shell
sudo dnf install ./*.rpm --allowerasing --nobest
yum install ./*.rpm --disablerepo=*
```

缺包时，如果 amd64 目录中有同名主包，通常说明 arm64 也必须补齐。可以到 openEuler 源查找：

<https://dl-cdn.openeuler.openatom.cn/openEuler-22.03-LTS-SP4/OS/aarch64/Packages/>

查询某个能力由哪个包提供：

```shell
dnf provides "pkgconfig(liblzma)"
```

如果冲突来自默认下载的 `-devel` 包，并且 amd64 目录也没有对应包，可以优先删除这些 `-devel` 包再验证。

最终发现系统自带 `libnetwork` 会与 Docker CE 依赖冲突，需要先移除：

```shell
dnf remove libnetwork
```

之后再让 BKE init 自动安装离线包。

