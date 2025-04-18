> 原文地址 [yangwenqing.com](https://yangwenqing.com/archives/1797/)

英特尔持续推动英特尔 ® 图形处理器上的图形虚拟化技术。这些技术中的最新技术是单根 IO 虚拟化 （SR-IOV），它取代了之前的 英特尔 ® Graphics Virtualization Technology –g （GVT-g）要了解每个[英特尔 ® 显卡家族支持哪种显卡](https://www.intel.cn/content/www/cn/zh/support/articles/000093216/graphics/processor-graphics.html)虚拟化技术，请参阅下表：  
`注意，11代Alchemist代号并不支持SR-IOV，也不支持GVT-g`  
[![](https://yangwenqing.com/usr/uploads/2024/05/742036566.png)](https://yangwenqing.com/usr/uploads/2024/05/742036566.png)

[支持列表](https://yangwenqing.com/usr/uploads/2024/05/742036566.png)

PVE 基本设置
--------

安装 PVE 过程略过，提前将 pve 安装好，推荐安装 PVE8 版本，如要正常使用 11-14 代核显 SR-IOV，需要将内核更新到`6.1`以上。并在 BIOS 提前开启`VT-d`,`SR-IOV`,`Above 4GB(如有可以开)`

本篇文章将大量使用`nano`文本编辑命令，至于怎么使用自行百度，这里不重复造轮子了。 知道如何保存就行`Ctrl +X` 输入 “`Y`” 回车保存

### 更换系统源

`国内清华源`

### 更换企业源

`国内清华源`

### 修复源 401 错误

### 执行更新软件源

### LXC 容器更源

`国内清华源`

### PVE 常用优化脚本

1) 去掉登录订阅提示  
2) 合并 local-lvm 以最大化利用硬盘空间  
3) 添加 CPU 频率硬盘温度  
4) 删掉不用的内核等信息  

intel 11-14 代开启 SRIOV 核显虚拟化
---------------------------

以下设置是 11-14 代开启 SRIOV 核显的设置，GVT-g 的往下翻。

### 安装 headers 和 firmware 并重启

### 开启硬件直通和 i915guc

需要提前在主板 BIOS 开启虚拟化功能，才能开启硬件直通。在 BIOS 开启`vt-d`和`SRIOV`  
使用`nano`命令编辑`/etc/default/grub`

### 加载内核模块

编辑`nano /etc/modules`

### 执行更新 initramfs

### 验证是否开启直通

出现如下例子。则代表成功

此时输入命令

### 拉取 i915-sriov 驱动

### 编译 i915-sriov 驱动

Linux i915-sriov 驱动程序目前只支持 6.1-6.5 内核，低于或者高于该阶段的内核请先进行升级或者降级内核。

经过漫长等待完成后，检查安装是否成功，运行

### 安装 sysfsutils

### 添加 VFs 核显数量

按需设置最高 7 个，设置 1 个时性能最强。

### 验证是否开启核显 SRIOV

使用 lspci 命令查看核显信息，有如下内容则成功开启核显  
`注意：物理核显02.0不能直通，否则所有虚拟核显消失`

### 创建核显 SR-IOV 的 Win 虚拟机参数

按以下虚拟机参数模板建立好虚拟机，打上核显驱动就能正常使用核显了。  

  
[![](https://yangwenqing.com/usr/uploads/2024/05/3175206401.png)SR-IOV 的 Win 虚拟机参数](https://yangwenqing.com/usr/uploads/2024/05/3175206401.png)  
[![](https://yangwenqing.com/usr/uploads/2024/05/566400418.png)i5-12500T 核显 SR-IOV 驱动正常](https://yangwenqing.com/usr/uploads/2024/05/566400418.png)

intel 6-10 代开启 GVT-g 核显虚拟化
--------------------------

### 开启硬件直通和 GVT-g

需要提前在主板 BIOS 开启虚拟化功能，才能开启硬件直通。在 BIOS 开启`vt-d`  
使用`nano`命令编辑`/etc/default/grub`

### 加载内核模块

编辑`nano /etc/modules`

### 执行更新 initramfs

### 验证是否开启核显 GVT-g

### 创建核显 GVT-g 的 Win 虚拟机参数

按以下虚拟机参数模板建立好虚拟机，打上核显驱动就能正常使用核显了。  

  
[![](https://yangwenqing.com/usr/uploads/2024/05/1580184855.png)GVT-g 的 Win 虚拟机参数](https://yangwenqing.com/usr/uploads/2024/05/1580184855.png)  
[![](https://yangwenqing.com/usr/uploads/2024/05/3943219954.png)i3-9100 核显 GVT-g 驱动正常](https://yangwenqing.com/usr/uploads/2024/05/3943219954.png)
