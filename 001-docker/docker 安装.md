## 内地

### docker

```bash
curl -fsSL get.docker.com -o get-docker.sh && sudo sh get-docker.sh --mirror Aliyun

docker -v  # 查看docker版本
systemctl enable docker  # 设置开机自动启动
```

### docker-compose

其实现在安装了docker，默认就会安装新版的`docker compose`，运行命令为：`docker compose`，安
```bash
# 查看版本
docker compose version
```

## 海外

### docker

```bash
wget -qO- get.docker.com | bash
docker -v  # 查看docker版本
systemctl enable docker  # 设置开机自动启动
```
