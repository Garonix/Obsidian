## 步骤

1. **准备 Swarm 集群**

   - 在主节点初始化 Swarm: `docker swarm init`
   - 在其他节点加入 Swarm: `docker swarm join`
2. **创建 docker-compose.yml 文件**

   - 定义应用服务、镜像、端口、副本数量等
   - 示例
```yaml
version: "3.9"
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    deploy:
      replicas: 3
      resources:
        limits:
          cpus: "1"
          memory: "512M"
      restart_policy:
        condition: on-failure
  redis:
    image: redis:latest
```

3. **部署到 Swarm**

   - 将 docker-compose.yml 文件上传到主节点
   - 执行部署命令: `docker stack deploy -c docker-compose.yml my-app`
     (`my-app` 为应用名称，可自定义)
4. **查看部署状态**

   - 查看应用部署情况: `docker stack ls`
   - 查看服务运行状态: `docker service ls`
5. **扩展应用**

   - 修改 docker-compose.yml 文件中的 `replicas` 数量
   - 重新部署: `docker stack deploy -c docker-compose.yml my-app`
6. **移除应用**

   - `docker stack rm my-app`

## 关键命令

- `docker swarm init`: 初始化 Swarm 集群
- `docker swarm join`: 加入 Swarm 集群
- `docker stack deploy -c docker-compose.yml <app_name>`: 部署应用到 Swarm
- `docker stack ls`: 查看应用部署情况
- `docker service ls`: 查看服务运行状态
- `docker stack rm <app_name>`: 移除应用

## 注意事项

- 确保 Docker 版本兼容
- 在 docker-compose.yml 中定义网络
- 设置资源限制，防止容器占用过多资源
