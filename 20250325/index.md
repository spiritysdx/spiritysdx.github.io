# 通过ProxmoxVE制作kubevirt可用的Windows镜像


## 通过ProxmoxVE制作kubevirt可用的WIN镜像

<https://github.com/ILLKX/Windows-VirtIO>

下载带virtio的虚拟机镜像

然后借鉴 <https://www.spiritlhl.net/guide/pve/pve_windows.html> 开设虚拟机，到图形化安装后即可，不要配置网络

在虚拟机内需要设置一个bat脚本，设置系统启动后执行，脚本的内容是

```powershell
$arpRetryCountPath = "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters"
Set-ItemProperty -Path $arpRetryCountPath -Name ArpRetryCount -Value  0
sleep 5
ipconfig /renew
```

不这么设置的话虚拟机在kubevirt中的dhcp获取不到正确的ip地址。

经验证，```Win2008```和```Win7```及更早版本的网络设置无效，注册表修改仅对更高版本系统有效。

设置完成后，需要重启电脑，手动执行命令的话是无法生效的，会报错获取到的IP地址冲突。

如果启动后发现有连接但是网不通，查看```ipconfig```后是dhcp自动获取到了ip，但ip与kubevirt配置中的ip不同，需要执行

```powershell
ipconfig /release
ipconfig /renew
```

可重新进行dhcp的ip获取，然后应该网络就被正确设置了。

#### 安装预制的工具

在导出镜像之前，安装所需的工具到C盘，再进行导出

比如安装 Visual Studio 在安装时要将安装的路径选到C盘下，默认就是C盘的不要修改

#### 设置虚拟机C盘自动扩容和D盘自动创建

设置虚拟机C盘自动扩容和D盘自动创建

由于原先系统制作的时候预定义的硬盘为20G，实际开设过程中需要增大硬盘大小，否则会出现C盘使用一段时间后磁盘写满的情况，所以需要在开设的时候指定扩容硬盘。

但这样依然没有自动扩容硬盘到C盘，进入后可以看到有未分配的硬盘，同时如果有挂载数据盘，且这个数据盘未初始化未有分区的话，需要进行自动格式化、分区创建和盘符创建。

C盘扩容的同时，由于使用的是ISO制作的虚拟机镜像，会存在恢复分区，才可以使用以下内容

```shell
for /f "tokens=2 delims=:" %%i in ('wmic logicaldisk where "DeviceID='C:'" get DeviceID ^| find ":"') do set drive=%%i
echo select disk 0 > diskpart_script.txt
echo list partition >> diskpart_script.txt
echo select partition 3 >> diskpart_script.txt
echo delete partition override >> diskpart_script.txt
echo select partition 2 >> diskpart_script.txt
echo extend >> diskpart_script.txt
diskpart /s diskpart_script.txt
del diskpart_script.txt
```

如果C盘分区后面没有跟着恢复分区，直接跟着未分配的分区，那么使用

```shell
set "dptemp=%temp%\extend_c_drive.txt"
> "%dptemp%" (
    echo select volume C
    echo extend
)
diskpart /s "%dptemp%"
del "%dptemp%"
```

就行了

需要在导出成kubevirt用的qcow2镜像之前创建一个bat脚本，创建定时任务在系统启动后执行。

下面这个脚本C盘分区扩容后面部分的内容，就是挂载了数据盘，初始化格式化创建D盘盘符的脚本。

```bat
@echo off
setlocal
net session >nul 2>&1
if %errorlevel% neq 0 (
    pause
    exit /b
)

for /f "tokens=2 delims=:" %%i in ('wmic logicaldisk where "DeviceID='C:'" get DeviceID ^| find ":"') do set drive=%%i
echo select disk 0 > diskpart_script.txt
echo list partition >> diskpart_script.txt
echo select partition 3 >> diskpart_script.txt
echo delete partition override >> diskpart_script.txt
echo select partition 2 >> diskpart_script.txt
echo extend >> diskpart_script.txt
diskpart /s diskpart_script.txt
del diskpart_script.txt

set "dpscript=%temp%\init_d_disk.txt"
> "%dpscript%" (
    echo select disk 1
    echo attributes disk clear readonly
    echo clean
    echo convert gpt
    echo create partition primary
    echo format fs=ntfs quick
    echo assign letter=D
)
diskpart /s "%dpscript%"
del "%dpscript%"
```

在 设置 -- 控制面板 -- 计划任务 -- 创建基本任务 -- 当前用户登录时启动任务(L) -- 选择这个bat程序文件 -- 创建任务

#### 开机自启程序

类似上述操作，事先查找到要开机自启动的桌面的程序路径，右键点开图标的```属性(R)```，复制粘贴```属性(R)```中的```目标(T)```的地址，替换下面的```"路径"```为对应的路径

```bat
@echo off
setlocal enabledelayedexpansion
start "" "路径"
```

#### 取消用户登录密码自动登录

注册表编辑器打开

```shell
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device
```

这个地址，将DevicePasswordLessBuildVersion的值改为0

然后打开控制面板，打开用户账户，或直接搜索栏搜索```netplwiz```，或```WIN+R```输入```netplwiz```。

将要使用本计算机，用户必须输入用户名和密码取消勾选。

然后会弹窗要求输入当前用户的密码，输入后点确认就设置成功了。

系统如果重启后不需要输入密码直接进入桌面就证明成功了。

#### 取消开机弹出服务器管理器

服务器管理器右上角 “管理”—“服务器管理器属性”

在弹出的选项卡内，把“在登录时不自动启动服务器管理器”前面的对钩打上，确定

再次重启服务器进行测试，发现不会弹出了。

#### 导出最终镜像

确保在虚拟机所在的节点的机器上存在```qemu-img```命令

```shell
which qemu-img
qemu-img -h
```

如果不存在需要进行下载。

然后需要先在平台上将虚拟机关机，避免进程占用文件。

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

