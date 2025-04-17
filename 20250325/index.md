# 通过ProxmoxVE制作kubevirt可用的Windows镜像


## 通过ProxmoxVE制作kubevirt可用的WIN镜像

https://github.com/ILLKX/Windows-VirtIO

下载带virtio的虚拟机镜像

然后借鉴 https://www.spiritlhl.net/guide/pve/pve_windows.html 开设虚拟机，到图形化安装后即可，不要配置网络

然后在部署机执行

```shell
qm stop 100
qm config 100
```

查询挂载盘所在地址，然后导出到当前路径

```shell
qemu-img convert -O qcow2 /var/lib/vz/images/100/vm-100-disk-0.qcow2 /root/win2022.qcow2
```

导出后执行

```shell
qemu-img info /root/win2022.qcow2
```

可查看文件大小和信息

该文件通过ACE的虚拟机管理启动win的虚拟机后，进入虚拟机

在虚拟机内需要设置一个bat脚本，设置系统启动后执行，脚本的内容是

```powershell
$arpRetryCountPath = "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters"
Set-ItemProperty -Path $arpRetryCountPath -Name ArpRetryCount -Value  0
sleep 5
ipconfig /renew
```

不这么设置的话虚拟机dhcp获取不到正确的ip地址。

经验证，Win2008 和 Win7 及更早版本的网络设置无效，注册表修改仅对更高版本系统有效。

设置完成后，需要重启电脑，手动执行命令的话是无法生效的，会报错获取到的IP地址冲突。

如果启动后发现有连接但是网不通，查看```ipconfig```后是dhcp自动获取到了ip，但ip与kubevirt配置中的ip不同，需要执行

```powershell
ipconfig /release
ipconfig /renew
```

可重新进行dhcp的ip获取，然后应该网络就被正确设置了。

### 安装预制的工具

在导出镜像之前，安装所需的工具到C盘，再进行导出

比如安装 Visual Studio 在安装时要将安装的路径选到C盘下，默认就是C盘的不要修改 

### 导出镜像

确保在虚拟机所在的节点的机器上存在```qemu-img```命令

```
which qemu-img
qemu-img -h
```

如果不存在需要进行下载。

然后需要先在平台上将虚拟机关机，避免进程占用文件。

然后导出这个配置好的镜像，pvc地址在

```shell
ls /opt/local-path-provisioner/
```

中，查看对应设置虚拟机名字的pve地址

将system的`disk.img`实际路径地址填入`<disk.img地址>`，即可导出镜像

(注意pvc路径名字不包含`data`，包含`system`)

```shell
qemu-img convert -f raw <disk.img地址> -O qcow2 win2022.qcow2
```

示例：

```
qemu-img convert -f raw /opt/local-path-provisioner/pvc-be0441e0-d818-49a7-adf4-be7003cf435a_ysss-ys2-1742284203062_system-pvc-win2022/disk.img -O qcow2 win2022_visual_studio.qcow2
```
