# 科研与小团队 GPU 共享容器完整方案：从零搭建 OneClickVirt 管理平台并通过 Agent 模式纳管内网 GPU 节点


## 背景与适用场景

科研团队或小型技术团队通常面临这样的情况：一台或几台装有 GPU 的物理机放在机房或实验室里，没有固定的独立公网 IPv4 地址，多位成员需要共享这台机器做实验，同时希望彼此环境隔离、互不干扰，还能从宿舍或任意外网地址访问自己的容器。

传统做法是直接在 GPU 机器上开多个账号，或者手动用 screen / tmux 隔离会话，这些方案既难以限制各人的资源用量，也无法做到环境完全隔离。

本文介绍的方案使用开源项目 [OneClickVirt](https://github.com/oneclickvirt/oneclickvirt)（后文简称 OCV）实现以下目标：

- 一台有独立公网 IPv4 的轻量服务器作为**管理主控**，部署 OCV 面板
- 一台无独立公网 IPv4 的内网 GPU 机器作为**受控节点**，通过 Agent 模式接入主控
- 主控与节点之间建立 WebSocket 隧道，实现**内网穿透**，将节点内部容器的端口暴露到主控的公网地址上
- 在 GPU 节点上通过 LXD/Incus 为每位用户开设独立的 GPU 共享容器

整体架构如下：

```
外部用户（宿舍/远程）
         |
    HTTPS/WSS
         |
   主控服务器（有独立公网 IPv4，1核1G起即可）
    - OCV 面板
    - WebSocket 中继
         |
    WS/WSS 隧道（Agent 保持长连接）
         |
   GPU 节点（无独立公网 IPv4，机房/实验室内网）
    - LXD/Incus 虚拟化环境
    - NVIDIA 驱动
    - OCV Agent
         |
   各用户独立容器（每人一个，共享 GPU，通过内网穿透对外暴露端口）
```

---

## 资源需求一览

在动手之前，先确认手头的资源：

| 角色 | 要求 |
|------|------|
| 主控服务器 | Linux，有独立公网 IPv4，Root 权限，1G+ 内存，10G+ 磁盘，**脚本部署或裸机编译**（不能用 Docker 部署 OCV，否则无法使用内网穿透） |
| GPU 节点 | Linux 系统节点，Root 权限，已安装 NVIDIA 驱动，已安装 LXD (Ubuntu系统) 或 Incus (Debian系统) |
| 域名（可有可无） | 一个指向主控服务器公网 IP 的域名，用于配置 HTTPS 和 WSS，配置后加密流量传输 |

---

## 第一步：在主控服务器上安装 OCV 面板

主控服务器是整个方案的核心，负责管理节点、分发容器、转发端口。

### 使用一键全栈安装脚本（推荐）

该脚本会自动安装数据库（MySQL/MariaDB）、反向代理（Caddy 或 Nginx）、TLS 证书、前端静态文件和后端服务。安装前确保主控服务器至少有 10G 可用磁盘和 2G 内存。

**国内服务器下载脚本**：

```shell
curl -fsSL https://cdn.spiritlhl.net/https://raw.githubusercontent.com/oneclickvirt/oneclickvirt/main/scripts/install_full.sh -o install_full.sh
```

**国际服务器下载脚本**：

```shell
curl -fsSL https://raw.githubusercontent.com/oneclickvirt/oneclickvirt/main/scripts/install_full.sh -o install_full.sh
```

**有域名时使用交互式安装**（脚本会逐步询问域名、邮箱、数据库类型、反向代理类型等，跟着提示填写即可）：

```shell
bash install_full.sh
```

**无域名纯 IP 访问的快速安装**：

```shell
bash install_full.sh \
  --non-interactive \
  --domain http://你的主控公网IP \
  --proxy caddy
```

安装完成后终端会打印数据库密码，**关闭终端前务必复制保存**，后续初始化面板时需要用到。

### 安装方式对比

| 安装方式 | 适用场景 | 特点 |
|----------|----------|------|
| 一键全栈安装脚本 | 裸金属快速部署 | 全自动安装数据库/反向代理/TLS/前后端，推荐 |
| 前后端分离手动部署 | 高性能需求 | 灵活但配置复杂 |
| 一体化二进制部署 | 本地无公网 IP 时快速体验 | 简单但性能较差 |
| Docker 部署 | 仅用于功能评估 | 快速但**不支持内网穿透**，不适合本文场景 |

**重要说明**：Docker 部署的 OCV 主控无法支持内网穿透功能，凡是需要纳管没有独立公网 IPv4 节点的场景，必须使用脚本或二进制方式部署主控。

---

## 第二步：初始化 OCV 面板

访问主控的域名（或 `http://主控IP`），首次打开会自动跳转到初始化页面。

![初始化界面](https://www.spiritlhl.net/assets/init.CCPKl32d.png)

填写数据库连接信息（安装脚本已自动创建名为 `oneclickvirt` 的数据库，密码即安装完成时输出的那个），以及管理员账号和密码，点击测试连接通过后点击初始化系统。

![初始化成功](https://www.spiritlhl.net/assets/init_success.DAbHwjWK.png)

初始化完成后自动跳转首页。

![面板首页](https://www.spiritlhl.net/assets/home.BlmfOIEU.png)

若使用默认配置进行初始化，默认管理员账号为 `admin`，密码为 `Admin123!@#`。

**初始化完成后必须做的事**：

1. 立即在用户管理页面修改管理员默认密码
2. 禁用或删除默认测试用户 `testuser`

---

## 第三步：在 GPU 节点上准备虚拟化环境

GPU 节点（即那台有 GPU 的机器）需要提前准备好两件事：确认 NVIDIA 驱动已正常工作，以及安装 LXD 或 Incus 容器化环境。

### 确认 GPU 驱动正常

在 GPU 节点上执行：

```shell
nvidia-smi
```

正常情况应该显示类似如下输出，记录 `Driver Version` 的具体版本号（例如 `535.171.04`），后续在容器内安装驱动时必须与此版本**完全一致**：

```
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.171.04             Driver Version: 535.171.04   CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA RTX A6000               Off | 00000000:61:00.0 Off |                  Off |
| 30%   42C    P0              83W / 300W |      0MiB / 49140MiB |      1%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+
```

如果 `nvidia-smi` 命令报错或不存在，需要先安装驱动，确认输出正常后再继续。

### 安装 LXD（Ubuntu 22.04 系统）

```shell
snap install lxd
apt install zfsutils-linux bridge-utils -y
```

### 初始化 LXD

```shell
/snap/bin/lxd init
```

初始化过程中有几个关键选项需要手动指定，其余直接回车使用默认值：

```
Name of the storage backend to use (ceph, dir, lvm, zfs, btrfs) [default=zfs]: zfs
Would you like to use an existing empty block device? (yes/no) [default=no]: no
Size in GiB of the new loop device [default=30GiB]: 根据本地磁盘空闲空间填写，有200G想分配给总的容器硬盘大小用，例如 200GiB
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]: no
```

初始化完成后，执行以下命令进行基本优化：

```shell
# 添加第三方镜像源
lxc remote add opsmaru https://images.opsmaru.dev/spaces/9bfad87bd318b8f06012059a --public --protocol simplestreams

# 设置 CGROUP 相关配置
snap set lxd lxcfs.loadavg=true
snap set lxd lxcfs.pidfd=true
snap set lxd lxcfs.cfs=true
systemctl restart snap.lxd.daemon

# 禁止镜像自动更新
lxc config set images.auto_update_interval 0

# 解除进程数限制
echo '*          hard    nproc       unlimited' >> /etc/security/limits.conf
echo '*          soft    nproc       unlimited' >> /etc/security/limits.conf
echo 'UserTasksMax=infinity' >> /etc/systemd/logind.conf

# 关闭防火墙
ufw disable
```

建议此时重启服务器，确认 LXD 服务正常后再继续。

---

## 第四步：通过 Agent 模式将 GPU 节点纳管到 OCV 面板

标准的节点纳管方式需要主控通过 SSH 直连到节点，这要求节点有固定的独立公网 IPv4。内网 GPU 机器不满足该条件，因此使用 **Agent 模式**：由节点主动向主控发起连接并建立隧道，主控无需主动连接节点。

### 在面板中新增节点并选择 Agent 模式

登录 OCV 管理后台，进入节点管理，点击新增节点，在节点类型选择界面中点击 **Agent 模式**。

![新增节点选择 Agent 模式](https://www.spiritlhl.net/assets/agent0.CfOi1y0_.png)

点击 Agent 模式后，进入基础信息配置页面。

![Agent 模式基础信息配置页面](https://www.spiritlhl.net/assets/agent1.B3mvgfB2.png)

### 填写节点配置

Agent 模式下，IP 地址和端口均为**可选字段**：

- **本地节点 / 家宽节点 / NAT 网络节点**（即本文的 GPU 节点场景）：IP 地址和端口**留空**
- **有固定独立公网 IPv4 的服务器**：可按实际情况填写

留空的意义：该节点通过 Agent 主动连接到主控，主控不会主动 SSH 到节点，因此不需要填 IP。

需要注意的是：

- IP 和端口**留空**时，后续开设容器的网络模式只能选**无端口映射模式**，容器端口需要通过后台手动添加内网穿透规则来暴露
- IP 和端口**已填写**时，网络模式与标准模式一致，可自由选择全部类型

![节点配置及网络模式说明](https://www.spiritlhl.net/assets/agent2.DH-1h9GP.png)

### 保存节点并获取 Agent 安装命令

配置完成后点击保存，页面的**连接配置**区域会出现生成安装命令的按钮。

关于 Token：节点保存后系统会为其生成唯一的 Token，该 Token **一经生成无法单独修改**。若 Token 泄露，只能删除该节点记录、重新创建节点。请妥善保管生成的安装命令。

![连接配置区域，点击生成安装命令](https://www.spiritlhl.net/assets/agent3.CBfCJ_DV.png)

点击生成后会得到一条完整的安装命令，复制该命令，到 **GPU 节点服务器**上以 Root 身份执行，等待安装完成即可。

### 验证连接状态

回到 OCV 管理后台，在该节点的连接配置页面下方点击**检测**按钮。

![点击检测按钮验证连接](https://www.spiritlhl.net/assets/agent4.BEG_w41m.png)

检测成功后界面如下：

![检测成功界面](https://www.spiritlhl.net/assets/agent7.BQIWOUed.jpeg)

至此，GPU 节点已成功纳管，后续操作均可在 OCV 面板中完成。

---

## 第五步：在 GPU 节点上开设 GPU 共享容器

### 选择网络模式

在对该节点开设容器时，由于 IP 和端口留空，网络模式必须选择**无端口映射**。

![网络模式选择无端口映射](https://www.spiritlhl.net/assets/agent5.K3I0SIuf.png)

选择无端口映射后，容器的外部端口访问需要通过后台的端口管理页面手动添加内网穿透规则。

### 推荐使用兑换码模式

在多人共用的场景下，推荐开启该节点的**仅兑换码兑换模式**，通过兑换码控制谁能在该节点上创建容器，避免资源被随意消耗。

管理员在后台的兑换码页面，创建兑换码时在设备选项中选择 GPU 设备（系统会自动识别节点上可用的 GPU），然后将兑换码通过普通用户视图兑换掉，容器即创建成功。

![兑换码页面选择 GPU 设备创建容器](https://www.spiritlhl.net/assets/gpu1.BqYtvfhm.jpeg)

### 添加内网穿透端口以便进入容器

容器创建成功后，需要为容器的 SSH 端口（22）添加内网穿透规则，才能从外部通过主控的公网地址访问到该容器。

进入管理员后台的**端口管理**页面，点击添加端口，端口类型选择**控制端转发（内网穿透）**，目标容器和目标端口填写对应值，外部端口留空由系统自动分配可用端口。

![端口管理页面，添加内网穿透端口](https://www.spiritlhl.net/assets/agent6.Hj3aLwzs.png)

添加成功后，可以直接通过管理后台的 Web SSH 功能进入该容器进行配置，也可以通过 `主控公网IP:分配的外部端口` 用标准 SSH 客户端连接。

![添加成功后通过 Web SSH 连接容器](https://www.spiritlhl.net/assets/gpu2.Ctk8v8IU.jpeg)

---

## 第六步：在容器内安装 GPU 驱动

这是本方案中最关键的一步。容器要能使用 GPU，必须在容器内安装**与宿主机版本完全一致**的 NVIDIA 驱动，并且安装时**不加载内核模块**（内核由宿主机管理，容器不应也无法加载新的内核模块）。

### 确认宿主机驱动版本

在第三步已经记录了宿主机的驱动版本，例如 `535.171.04`，容器内必须安装完全相同版本的驱动。

可以在宿主机上再次确认：

```shell
nvidia-smi | grep "Driver Version"
```

### 通过 Web SSH 或外部端口进入容器

通过 OCV 面板的 Web SSH 功能，或使用 SSH 客户端连接：

```shell
ssh root@主控公网IP -p 系统分配的外部端口
```

### 在容器内安装编译依赖

```shell
apt update && apt install -y build-essential wget
```

### 下载对应版本的驱动安装包

访问 NVIDIA 官方驱动下载页面：[https://www.nvidia.cn/Download/Find.aspx?lang=cn](https://www.nvidia.cn/Download/Find.aspx?lang=cn)

按照宿主机的显卡型号和操作系统进行搜索，找到与宿主机驱动版本完全一致的 `.run` 安装文件。在搜索结果页面，**右键**点击"同意并开始下载"按钮，复制下载链接，然后**在容器内**用 wget 下载（注意是在容器内，不是宿主机）：

```shell
wget https://cn.download.nvidia.com/XFree86/Linux-x86_64/535.171.04/NVIDIA-Linux-x86_64-535.171.04.run
```

将 `535.171.04` 替换为实际的宿主机驱动版本号。

### 安装驱动，必须加入 --no-kernel-module 参数

```shell
bash NVIDIA-Linux-x86_64-535.171.04.run --no-kernel-module
```

安装过程中所有交互选项均选择 **no** 即可。

### 验证 GPU 共享是否成功

安装完成后在容器内执行：

```shell
nvidia-smi
```

若输出与宿主机版本一致，则 GPU 已成功共享到该容器中。

![容器内 nvidia-smi 输出正常，GPU 共享成功](https://www.spiritlhl.net/assets/gpu3.DsMLQ_4f.jpg)

**注意**：由于驱动未加载进内核，容器重启后若 `nvidia-smi` 异常，重新执行上述安装命令再重启容器即可恢复。请保留下载好的 `.run` 文件，不要删除。

---

## 第七步：防止驱动被系统自动更新破坏

驱动版本固定非常重要，一旦宿主机或容器的驱动被系统自动更新到不同版本，共享将失效。在**宿主机和每个容器内**分别执行以下操作：

```shell
cat > /etc/apt/apt.conf.d/10periodic << 'EOF'
APT::Periodic::Update-Package-Lists "0";
APT::Periodic::Download-Upgradeable-Packages "0";
APT::Periodic::AutocleanInterval "0";
APT::Periodic::Unattended-Upgrade "0";
EOF
```

在**宿主机上**额外锁定 NVIDIA 相关包版本：

```shell
apt-mark hold nvidia-*
```

---

## 第八步：制作母本容器并批量复制给多位用户

为每位用户单独从零安装驱动非常低效。正确做法是以当前配置好 GPU 驱动的容器为**母本**，批量复制出多个容器分配给不同用户。

1. 停止当前已配置好的容器（在 OCV 面板操作）
2. 在 OCV 面板的兑换码页面，创建兑换码时选择**复制模式**，指定该已停止的容器为源容器
3. 批量生成兑换码，每位用户兑换一张，得到独立的 GPU 容器
4. 为每个新容器单独在端口管理页面添加内网穿透规则，分配各自的外部 SSH 端口
5. 将 `主控公网IP:各自的外部端口` 分发给对应用户，用户即可 SSH 进入自己的容器

每个复制出的容器均带有 GPU 访问能力，各自环境完全隔离，互不干扰。

---

## 内网穿透安全配置

### WebSocket 反向代理配置

主控与 Agent 节点之间的内网穿透通过 WS/WSS WebSocket 隧道实现。若主控前置了 Nginx 或 OpenResty，反向代理中必须配置 WebSocket 协议升级，否则隧道无法建立。

Nginx/OpenResty 配置示例（添加到站点的反代配置中）：

```nginx
location /api/v1/ws/ {
    proxy_pass http://127.0.0.1:8888;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_buffering off;
    proxy_read_timeout 3600s;
    proxy_send_timeout 3600s;
}

location /api {
    proxy_pass http://127.0.0.1:8888;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_http_version 1.1;
    proxy_buffering off;
    proxy_read_timeout 600s;
    proxy_send_timeout 600s;
}
```

使用一键全栈安装脚本部署的主控，反向代理配置已自动生成，无需手动调整。

### 可选使用 WSS 加密隧道

若节点有防探测或隧道保密需求，强烈建议：

1. 为主控绑定域名（使用 Caddy 可自动申请 TLS 证书）
2. 纳管节点时确保安装命令中 `--url` 参数使用 `wss://你的域名` 而不是 `ws://` 明文协议

WSS 通过 TLS 加密隧道流量，有效降低隧道被识别或封禁的风险。但一般的没有保密需求的，WS协议也够用了，agent和主控通信不是FRP模式，本身已经具有一定的伪装特性了。

---

## 版本更新说明

当 OCV 主控版本升级后，需同步更新各 Agent 节点上的 Agent 程序。

在 OCV 面板中重新为对应节点生成安装命令，执行时将 `--source` 参数设置为 `controller`，Agent 会直接从主控服务器下载最新文件，无需依赖外部网络，适合内网环境下的更新。

---

## 完整操作流程总结

| 步骤 | 操作位置 | 内容 |
|------|----------|------|
| 1 | 主控服务器 | 用一键脚本安装 OCV 面板 |
| 2 | 浏览器 | 打开 OCV 面板，完成数据库初始化，修改默认密码 |
| 3 | GPU 节点 | 确认 nvidia-smi 正常，安装并初始化 LXD |
| 4 | OCV 面板 | 新增节点，选择 Agent 模式，IP 和端口留空，保存并生成安装命令 |
| 5 | GPU 节点 | 执行 OCV 面板生成的 Agent 安装命令 |
| 6 | OCV 面板 | 对该节点点击检测，确认连接成功 |
| 7 | OCV 面板 | 创建兑换码时选择 GPU 设备，兑换出第一个 GPU 容器（网络模式选无端口映射） |
| 8 | OCV 面板 | 端口管理页面为容器添加控制端转发（内网穿透）SSH 端口 |
| 9 | 容器内 | 安装 build-essential，下载与宿主机版本一致的 NVIDIA 驱动 `.run` 文件 |
| 10 | 容器内 | 执行 `bash NVIDIA-*.run --no-kernel-module`，验证 nvidia-smi 正常 |
| 11 | 宿主机+容器 | 禁止 apt 自动更新，宿主机上锁定 nvidia-* 包版本 |
| 12 | OCV 面板 | 停止已配置好的容器，以其为母本通过兑换码复制模式批量开设容器 |
| 13 | OCV 面板 | 为每个新容器添加独立的内网穿透 SSH 端口，分发给对应用户 |

---

## 常见问题

**Q：Agent 安装命令执行后，主控检测一直失败怎么办？**

检查 GPU 节点是否能访问主控的域名或 IP（可在节点上用 `curl` 测试），确认主控反向代理已正确配置 WebSocket 支持，确认主控防火墙未拦截相关端口。

**Q：容器内安装驱动后重启容器，nvidia-smi 又报错了怎么办？**

因为 `--no-kernel-module` 方式安装的驱动不持久加载进内核，重新执行同一条安装命令（`bash NVIDIA-*.run --no-kernel-module`）后再重启容器即可恢复。这也是为什么要保留下载好的 `.run` 文件不要删除。

**Q：多个容器共享同一张 GPU 会互相影响吗？**

GPU 显存是共享的，各容器使用的显存总和不能超过物理显存上限。CPU 和内存可在 OCV 面板开设容器时配置上限，实现资源隔离。

**Q：能否同时纳管多台 GPU 节点？**

可以，每台 GPU 节点按照第三步到第六步各自独立操作一遍即可，OCV 面板支持管理多个节点，节点之间独立互不影响。

**Q：主控服务器没有 10G 磁盘怎么办？**

10G 磁盘是一键全栈安装脚本的最低要求（含数据库、日志等）。若磁盘不足，可改用一体化二进制部署方式（占用更小），或使用已有的外部 MySQL 数据库减少主控本身的磁盘占用。主控服务器本身不运行容器，1 核 1G 内存、5G 以上磁盘即可基本运行。

