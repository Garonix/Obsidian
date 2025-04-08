## Docker 使用代理拉取镜像

**问题:** 国内 Docker 拉取镜像慢，因为 Docker Hub 在国外。

**错误方法:**
仅在终端设置 `http_proxy` 和 `https_proxy` 环境变量。

**原因:** 这些环境变量只对 Docker 客户端(命令行工具)生效，对真正负责拉取镜像的 Docker Daemon(后台服务)无效。

**正确方法: 配置 Docker Daemon 的代理**

### 步骤:

1. **创建目录:**

```bash
mkdir -p /etc/systemd/system/docker.service.d
```

2. **创建配置文件:**

```bash
nano /etc/systemd/system/docker.service.d/http-proxy.conf
```

3. **写入配置 (替换为你的代理地址):**

```bash
[Service]
Environment="HTTP_PROXY=http://192.168.3.2:11088"
Environment="HTTPS_PROXY=http://192.168.3.2:11088"
```

4. **重新加载配置并重启 Docker:**

```bash
systemctl daemon-reload
systemctl restart docker
```

**核心总结:** 要让 `docker pull` 走代理，**必须配置 Docker Daemon 的代理**，而不是仅仅配置终端。
