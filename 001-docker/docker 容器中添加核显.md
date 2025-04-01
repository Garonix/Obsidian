# Docker 容器中添加核显支持

## 配置方法

### docker-compose方式

```yaml
devices:
  - /dev/dri:/dev/dri
privileged: true
```

### docker run方式

```bash
docker run -d --device=/dev/dri:/dev/dri --privileged your-image-name
```

## 验证方法

```bash
# 检查设备
ls -la /dev/dri/

# Intel核显
apt-get update && apt-get install -y intel-gpu-tools
intel_gpu_top

# AMD核显
apt-get update && apt-get install -y radeontop
radeontop
```

## 常见问题

- 权限问题: 添加 `group_add: [video]`
- 确保主机和容器内安装正确驱动
