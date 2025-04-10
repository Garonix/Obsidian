经测试，可适用于PVE 8.x，其他版本如果使用相同的网络配置方式，那就通用。

IPv6的配置适合于国内普通家宽IPv6前缀动态获取的情况。首先要保证路由器上配置正确，比如我的情况：

![](https://cokebar.info/wp-content/uploads/2024/01/Asus-Router-IPv6.png)

配置之前，备一套键盘+显示器是必须的，这样万一没配置好，导致断网/系统进不去的时候，可以物理访问主机解决问题。

连接PVE物理机，复制 /etc/network/interface 文件到 /etc/network/interface.new ，打开 interface.new 文件，内容大概如下：

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16|auto lo<br><br>iface lo inet loopback<br><br>iface enp89s0 inet manual<br><br>auto vmbr0<br><br>iface vmbr0 inet static<br><br>address 192.168.1.2/24<br><br>gateway 192.168.1.1<br><br>bridge-ports enp89s0<br><br>bridge-stp off<br><br>bridge-fd 0<br><br>iface enp87s0 inet manual<br><br>source /etc/network/interfaces.d/*|

其中enp89s0是物理机插入网线的接口，pve默认建立一个网桥vmbr0桥接到enp89s0，enp89s0并不配置TCP/IP协议，TCP/IP协议配置在网桥vmbr0上。默认是采用静态地址。

配置ipv6，默认vmbr0是没有获取上游给的公网ipv6的，只有自动分配的fe80::开头的私有地址，无法访问公网ipv6网络。在配置文件内增加如下内容：

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4|iface vmbr0 inet6 auto<br><br>dhcp 1<br><br>accept_ra 2<br><br>request_prefix 1|

这里的配置是参考的Debian官方的文档：[https://wiki.debian.org/IPv6PrefixDelegation](https://wiki.debian.org/IPv6PrefixDelegation)

最终配置文件大概这样：

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16<br><br>17<br><br>18<br><br>19<br><br>20|auto lo<br><br>iface lo inet loopback<br><br>iface enp89s0 inet manual<br><br>auto vmbr0<br><br>iface vmbr0 inet static<br><br>address 192.168.1.2/24<br><br>gateway 192.168.1.1<br><br>bridge-ports enp89s0<br><br>bridge-stp off<br><br>bridge-fd 0<br><br>iface vmbr0 inet6 auto<br><br>dhcp 1<br><br>accept_ra 2<br><br>request_prefix 1<br><br>iface enp87s0 inet manual<br><br>source /etc/network/interfaces.d/*|

修改好以后，重新进到“系统”-“网络”界面，刷新一下，可以看到下图的情况，点击“应用配置”，稍等一段时间。

![](https://cokebar.info/wp-content/uploads/2024/01/pve-network-settings.png)

用pve主机shell运行ifconfig命令看一下结果，可以看到inet6已经获取到了240e开头的公网ipv6地址了。

![](https://cokebar.info/wp-content/uploads/2024/01/pve-ipv6.png)

最后再试一下网站访问，运行 curl -6 test6.ustc.edu.cn 看能否正常输出网页html：![](https://cokebar.info/wp-content/uploads/2024/01/pve-ipv6-test.png)

如上图，一切正常，OK搞定。

PS：顺道附一下把PVE默认的Debian官方apt源替换成清华源的结果：

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16<br><br>17<br><br>18<br><br>19<br><br>20<br><br>21<br><br>22<br><br>23<br><br>24|# deb http://ftp.debian.org/debian bookworm main contrib<br><br># deb http://ftp.debian.org/debian bookworm-updates main contrib<br><br># security updates<br><br># deb http://security.debian.org bookworm-security main contrib<br><br>deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription<br><br>deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware<br><br># deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware<br><br>deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware<br><br># deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware<br><br>deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware<br><br># deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware<br><br>deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware<br><br># deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware|

另外还有一个问题，在启用intel AMT的远程管理功能后，由于AMT系统和PVE系统共用网卡，同一个MAC地址，AMT功能通讯时，PVE的日志就会出现大量这样的提示：

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5|Feb 02 00:00:02 pve kernel: vmbr0: received packet on enp89s0 with own address as source address (addr:ab:cd:ef:12:34:56, vlan:0)<br><br>Feb 02 00:00:07 pve kernel: vmbr0: received packet on enp89s0 with own address as source address (addr:ab:cd:ef:12:34:56, vlan:0)<br><br>Feb 02 00:00:13 pve kernel: vmbr0: received packet on enp89s0 with own address as source address (addr:ab:cd:ef:12:34:56, vlan:0)<br><br>Feb 02 00:00:18 pve kernel: vmbr0: received packet on enp89s0 with own address as source address (addr:ab:cd:ef:12:34:56, vlan:0)<br><br>Feb 02 00:00:23 pve kernel: vmbr0: received packet on enp89s0 with own address as source address (addr:ab:cd:ef:12:34:56, vlan:0)|

此时就需要给PVE系统内的对应网卡指定一个新的MAC地址：

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16<br><br>17<br><br>18<br><br>19<br><br>20<br><br>21|auto lo<br><br>iface lo inet loopback<br><br>iface enp89s0 inet manual<br><br>hwaddress ether ab:cd:ef:12:34:57<br><br>auto vmbr0<br><br>iface vmbr0 inet static<br><br>address 192.168.1.2/24<br><br>gateway 192.168.1.1<br><br>bridge-ports enp89s0<br><br>bridge-stp off<br><br>bridge-fd 0<br><br>iface vmbr0 inet6 auto<br><br>dhcp 1<br><br>accept_ra 2<br><br>request_prefix 1<br><br>iface enp87s0 inet manual<br><br>source /etc/network/interfaces.d/*|