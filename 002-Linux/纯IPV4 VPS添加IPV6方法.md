> 原文地址 [p3terx.com](https://p3terx.com/archives/use-he-tunnel-broker-to-add-public-network-ipv6-support-to-ipv4-vps-for-free.html)

![](https://imgcdn.p3terx.com/post/20210225051254.jpg)

## 前言

[Hurricane Electric](https://p3terx.com/go/aHR0cHM6Ly9oZS5uZXQ) (简称：`HE`) 提供免费的 IPv6 隧道服务，可以给只有 IPv4 的 VPS 添加公网 IPv6 地址。

## HE IPv6 隧道局限性

* 无法接入 Cloudflare CDN
* IP 评分机构标记为高风险，可能导致：
  * 网站限制访问或频繁人机验证
  * 银行购物网站判定为欺诈行为，导致封号

## 设置教程

### 创建 IPv6 隧道

1. 注册 [Tunnel Broker](https://p3terx.com/go/aHR0cHM6Ly93d3cudHVubmVsYnJva2VyLm5ldA) 账号
2. 点击 `Create Regular Tunnel`
3. 输入 VPS 公网 IP，选择就近节点
4. 点击 `Create Tunnel`

![](https://imgcdn.p3terx.com/post/20210214034651.png)

在 **Tunnel Details** 页面可以看到隧道信息，其中 **Client IPv6 Address** 是获得的公网 IPv6 地址。

### 添加 IPv6 网络接口

将配置添加到 `/etc/network/interfaces.d/` 目录：

```bash
sudo tee /etc/network/interfaces.d/he-ipv6 <<EOF
auto he-ipv6
iface he-ipv6 inet6 v4tunnel
address 2001:xxx:xxxx:xxxx::2
netmask 64
endpoint 216.66.84.46
local 233.xxx.xxx.233
ttl 255
gateway 2001:xxx:xxxx:xxxx::1
EOF
```

> **提示:** NAT VPS 需将 `local` 字段替换为内网 IP：`ip route get 8.8.8.8 | grep -oP 'src \K\S+'`

### 启用 IPv6 隧道

安装工具并启动网络接口：

```bash
sudo apt update
sudo apt install net-tools iproute2 -y
sudo ifup he-ipv6
```

若提示接口未知，添加：`echo 'source /etc/network/interfaces.d/*' >>/etc/network/interfaces` 后重试

检查接口：`ifconfig` 应该能看到 `he-ipv6` 接口

如未生效，重启网络：`sudo systemctl restart networking`

## 其它配置

### 检测 IPv6

执行 `ping6 google.com`，能通则表示 IPv6 已生效

### NAT VPS 额外设置

```bash
ufw allow 41
route -A inet6 add ::/0 dev he-ipv6
```

### DNS 设置优化

编辑 `/etc/resolv.conf`，使用支持 AAAA 记录的 DNS：

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

### 优先使用 IPv4 网络

```bash
echo 'precedence ::ffff:0:0/96 100' | sudo tee -a /etc/gai.conf
```

执行 `curl ip.p3terx.com` 应显示 IPv4 地址

### 删除 IPv6 隧道

```bash
sudo ifdown he-ipv6
sudo rm -f /etc/network/interfaces.d/he-ipv6
```
