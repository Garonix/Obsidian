> 原文地址 [www.cnblogs.com](https://www.cnblogs.com/milton/p/17565297.html)

安装
==

安装

```shell
sudo apt install samba
```

检查服务状态

```shell
systemctl status smbd --no-pager -l
```

检查是否启用 (开机自启动)

```shell
systemctl is-enabled smbd
# enable it if inactive
systemctl enable smbd
```

配置
==

(可选) 添加用户

```shell
sudo usermod -aG sambashare [username]
```

设置密码

```shell
sudo smbpasswd -a [username]
```

创建公开共享 (可匿名访问)

```shell
sudo vi /etc/samba/smb.conf
```

按以下格式创建内容

```yaml
[disk1]  
  path = /srv/disk1
  browsable =yes  
  create mask = 0770  
  directory mask = 0770
  writable = yes  
  guest ok = no
```

说明:

1.  `[public]`方括号内为 share 后显示的目录名
2.  `path = /data/` 为用于 share 的本地路径
3.  `browsable =yes` 是否可以浏览
4.  `create mask = 0660`
5.  `directory mask = 0771`
6.  `writable = yes`是否可写
7.  `guest ok = yes`是否允许匿名访问

开启 SMB1
-------

对于很多电视盒子, 运行安卓或 CoreElEC, 只支持 SMB1, 连接默认配置的 Samba 服务会直接报 Timeout, 需要开启 smbd 服务对 SMB1 的支持

编辑 /etc/samba/smb.conf, 在`[global]`下添加

```shell
## Enable SMB1 ##   
server min protocol = NT1
```

重启 smbd 后, 电视盒就可以连接了.

配置项
===

Samba 可以配置的属性可以参考 [https://www.samba.org/samba/docs/using_samba/ch08.html](https://www.samba.org/samba/docs/using_samba/ch08.html)

create mask
-----------

八进制数, 每个数 3-bit 代表一组权限 [rwx], 整个 mask 代表 smb 客户端在创建文件时可以设置哪些 bit.  
默认是 0744, 对应 [--- rwx r-- r--] 这代表着 unix 下的文件所有者可以 rwx [读, 写, 执行], 同组用户和其它可以 r [读]

对于下面的例子, create mask 限制从 smb 创建文件 / 目录时, 不管来自于什么客户端, 文件和目录的最大权限为 744

```
[data]    
  path = /home/samba/data    
  browseable = yes    
  guest ok = yes    
  writeable = yes    
  create mask = 744
```

If you need to change it for nonexecutable files, we recommend 0644, or rw-r--r--.

Keep in mind that the **execute bits can be used by the server to map certain DOS file attributes**, as described earlier. If you're altering the create mask, those bits have to be part of the create mask as well.

directory mask
--------------

与 create mask 相同, 八进制数, 每个数 3-bit 代表一组权限 [rwx], 整个 mask 代表 smb 客户端在创建目录时可以设置哪些 bit.  
默认是 0744, 对应 [--- rwx r-- r--], 允许所有用户读, 但是只允许所有者自己浏览和修改. 建议改为 0750, [--- rwx r-x ---], 避免所有人可以访问

下面的例子中, 从客户端创建的目录, 最大权限为 755

```
[data]    
  path = /home/samba/data    
  browseable = yes    
  guest ok = yes    
  writeable = yes    
  directory mask = 755
```

force create mode
-----------------

这个配置项用于 当文件权限发生变化时强制设置的权限位. 常用于设置文件默认的组权限. 这个配置也可以用于设置 DOS 属性: archive (0100), system (0010), or hidden (0001).

**TIP**

有些 windows 应用保存文件时, 会创建一个带. bak 后缀的文件, 当这些文件在 samba 共享目录下时, 所有者不一定是当前用户, 为了让当前用户还可以编辑修改, 可以设置 force create mode = 0660 , 这样可以保证新文件也可以被同组用户编辑.

force directory mode
--------------------

这个配置项用于 当目录权限发生变化时强制设置的权限位. 常用于设置组权限, 默认为 0000.

force group
-----------

这个配置用于给所有连接上的客户端, 只要客户端成功通过验证, 都会被指定一个固定的分组. 这个分组会体现在新创建的文件和目录上.

force user
----------

同样的, 这个配置会给连接上的验证通过的客户端, 指定一个用户, 体现在新创建的文件和目录上.

关于 CIFS 和 SMB 的选择
=================

In this day and age, you should always use the acronym SMB. I know what you’re thinking – “but if they’re essentially the same thing, why should I always use SMB?”

Two reasons:

1.  The CIFS implementation of SMB is rarely used these days. Under the covers, most modern storage systems no longer use CIFS, they use SMB 2 or SMB 3. In the Windows world, SMB 2 has been the standard as of Windows Vista (2006) and SMB 3 is part of Windows 8 and Windows Server 2012.
2.  CIFS has a negative connotation amongst pedants. SMB 2 and SMB 3 are massive upgrades over the CIFS dialect, and storage architects who are near and dear to file sharing protocols don’t appreciate the misnomer. It’s kind of like calling an executive assistant a secretary.

参考
==

*   [https://www.varonis.com/blog/cifs-vs-smb](https://www.varonis.com/blog/cifs-vs-smb)
*   [https://linuxconfig.org/how-to-configure-samba-server-share-on-ubuntu-22-04-jammy-jellyfish-linux](https://linuxconfig.org/how-to-configure-samba-server-share-on-ubuntu-22-04-jammy-jellyfish-linux)
*   [https://linux.how2shout.com/how-to-install-samba-on-ubuntu-22-04-lts-jammy-linux/](https://linux.how2shout.com/how-to-install-samba-on-ubuntu-22-04-lts-jammy-linux/)
*   Samba 配置 [https://wiki.samba.org/index.php/Setting_up_Samba_as_a_Standalone_Server#Creating_a_Basic_guest_only_smb.conf_File](https://wiki.samba.org/index.php/Setting_up_Samba_as_a_Standalone_Server#Creating_a_Basic_guest_only_smb.conf_File)