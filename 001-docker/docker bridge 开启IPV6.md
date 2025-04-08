## Docker Bridge 开启 IPv6

### 1. 编辑配置文件

编辑 `/etc/docker/daemon.json` 文件：

```shell
vi /etc/docker/daemon.json
```

添加以下内容（如果文件不存在，直接创建）：

```json
{  
    "log-driver": "json-file",  
    "log-opts": {  
        "max-size": "20m",  
        "max-file": "3"  
    },  
    "ipv6": true,  
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",  
    "experimental": true,  
    "ip6tables": true
}
```

### 2. 重启 Docker 服务

```shell
service docker restart
```

### 3. 验证 IPv6 状态

查看 Docker 网络 IPv6 状态：

```shell
docker network inspect bridge | grep EnableIPv6
```

### 4. 测试 IPv6 连接

```shell
docker run --rm -it busybox ping -6 -c4 www.google.com
```

### 5. 容器网络配置

注意：容器必须使用 `bridge` 网络模式才能启用 IPv6：

```yaml
network_mode: bridge
```
