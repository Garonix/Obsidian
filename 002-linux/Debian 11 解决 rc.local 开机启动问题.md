> 原文地址 [u.sb](https://u.sb/debian-rc-local/)

本文适用于 Debian 9/10/11。

## rc.local 开机启动问题解决

Debian 9 开始默认不带 `/etc/rc.local` 文件，但 `rc.local` 服务仍然存在。对于简单脚本，添加命令到 `/etc/rc.local` 比编写 systemd 服务更方便。

查看 rc-local 服务配置：

```bash
cat /lib/systemd/system/rc-local.service
```

检查服务状态：

```bash
systemctl status rc-local
```

### 解决步骤

1. 创建 rc.local 文件

```bash
cat <<EOF >/etc/rc.local
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# By default this script does nothing.

exit 0
EOF
```

2. 添加执行权限

```bash
chmod +x /etc/rc.local
```

3. 启用服务

```bash
systemctl enable --now rc-local
```

系统可能会显示警告（无需关注）。在 `/etc/rc.local` 中添加启动命令时，将命令放在 `exit 0` 前面，重启后生效。

## Linux 其他开机启动方式

### 1. systemd 服务（推荐）

创建服务文件：

```bash
sudo nano /etc/systemd/system/my-service.service
```

基本服务配置：

```ini
[Unit]
Description=My Custom Service
After=network.target

[Service]
Type=simple
ExecStart=/path/to/script.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

启用服务：

```bash
sudo systemctl enable my-service
sudo systemctl start my-service
```

### 2. crontab @reboot

使用 crontab 的特殊时间标记：

```bash
sudo crontab -e
@reboot /path/to/script.sh
```

### 3. /etc/init.d/ 和 update-rc.d

传统 SysV 初始化方式，创建脚本后执行：

```bash
sudo chmod +x /etc/init.d/myscript
sudo update-rc.d myscript defaults
```

### 4. 用户环境启动

- `/etc/profile` 或 `/etc/bash.bashrc`：适用于需要在用户环境中运行的程序

### 5. udev 规则

用于硬件事件触发：

```bash
# /etc/udev/rules.d/99-local.rules
ACTION=="add", SUBSYSTEM=="usb", RUN+="/path/to/script.sh"
```

选择建议：系统服务用 systemd，简单命令用 rc.local 或 @reboot。
