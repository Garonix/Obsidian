PVE NTP时间同步, 为客户机提供RTC时钟。

## 设置时区 [🔗︎](#设置时区)

|   |   |
|---|---|
|```<br>1<br>2<br>3<br>4<br>```|```<br># 查看可用时区<br>timedatectl list-timezones<br><br>timedatectl set-timezone "Asia/Shanghai"<br>```|

## 同步时间 [🔗︎](#同步时间)

|   |   |
|---|---|
|```<br>1<br>```|```<br>apt install systemd-timesyncd<br>```|

## 指定服务器 [🔗︎](#指定服务器)

vi /etc/systemd/timesyncd.conf

|   |   |
|---|---|
|```<br>1<br>```|```<br>NTP=ntp.aliyun.com ntp1.aliyun.com ntp2.aliyun.com ntp3.aliyun.com pool.ntp.org 0.asia.pool.ntp.org<br>```|

## 生效 [🔗︎](#生效)

|   |   |
|---|---|
|```<br>1<br>2<br>3<br>4<br>5<br>```|```<br>timedatectl set-ntp true<br>systemctl restart systemd-timesyncd<br><br>#查看状态使用<br>timedatectl timesync-status<br>```|

## Windows 客户机生效 [🔗︎](#windows-客户机生效)

### NT5 (XP及更老的系统) [🔗︎](#nt5-xp及更老的系统)

在 boot.ini 启动项后添加 `/usepmtimer`

### NT6+ (Vista及更新的系统) [🔗︎](#nt6-vista及更新的系统)

|   |   |
|---|---|
|```<br>1<br>```|```<br>bcdedit /set {default} USEPLATFORMCLOCK on<br>```|