>  原文地址 [halysl.github.io](https://halysl.github.io/2020/05/18/ubuntu%E5%90%AF%E5%8A%A8%E9%A1%B5%E9%9D%A2%E9%85%8D%E7%BD%AE%E7%BD%91%E7%BB%9C%E6%85%A2/)

开机过程中会出现，

```
[...]a start job is running for wait for network to be configured...[1 min 3 sec / no limit]
```

一般需要等待两到三分钟。这是因为一直无法配置，一般网口会通过 DHCP 或者其他协议自动获取配置，但是如果配置文件就没写相关信息，或者系统里的网卡其实是虚拟网卡，就无法自动获取，卡到最后还会报个错。

解决的思路主要有两个，一个是将 systemd-networkd-wait-online.service 服务停掉；一个是配置文件里改成 optional: true。

```
sudo systemctl mask systemd-networkd-wait-online.service
```
或者
```
systemctl disable NetworkManager-wait-online.service
```

```
$ networkctl
IDX LINK             TYPE               OPERATIONAL SETUP
  1 lo               loopback           carrier     unmanaged
  2 enp96s0f0        ether              no-carrier  configuring
  3 enp96s0f1        ether              routable    configured

3 links listed.

# 看到 enp96s0f0 无法配置，那么就在配置文件里帮它配置上
```

```
$ vi /etc/netplan/01-netcfg.yaml

# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    enp96s0f0:  # device name
      optional: true  # 加上这行
    enp96s0f1:
      addresses: [ 192.168.1.100/24 ]
      gateway4: 192.168.1.1
      nameservers:
          addresses:
              - "192.168.1.1"
```

```
$ sudo netplan generate && sudo netplan apply
```