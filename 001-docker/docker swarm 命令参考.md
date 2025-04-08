
`swarm` 集群由管理节点（`manager`）和工作节点（`work node`）构成。

- **`swarm manager`**：负责整个集群的管理工作包括集群配置、服务管理等所有跟集群有关的工作。
- **`work node`**：即可用节点，主要负责运行相应的服务来执行任务（`task`）。

## 集群管理命令

| 命令                        | 描述                                          | 示例                                                            |
| --------------------------- | --------------------------------------------- | --------------------------------------------------------------- |
| `docker swarm init`       | 初始化一个 Swarm 集群，将当前节点设置为主节点 | `docker swarm init --advertise-addr 192.168.1.100`            |
| `docker swarm join`       | 将一个节点加入到 Swarm 集群中                 | `docker swarm join --token <worker_token> 192.168.1.100:2377` |
| `docker swarm inspect`    | 查看 Swarm 集群的详细信息                     | `docker swarm inspect`                                        |
| `docker swarm update`     | 更新 Swarm 集群的配置                         | `docker swarm update --auto-lock-rotate-interval 1h`          |
| `docker swarm join-token` | 获取加入 Swarm 集群所需的令牌                 | `docker swarm join-token worker`                              |
| `docker swarm leave`      | 从集群中离开                                  | `docker swarm leave --force`                                  |
| `docker info`             | 查看 Docker 的系统信息，包括 Swarm 集群信息   | `docker info`                                                 |

## 节点管理命令

| 命令                              | 描述                        | 示例                                           |
| --------------------------------- | --------------------------- | ---------------------------------------------- |
| `docker node ls`                | 列出 Swarm 集群中的所有节点 | `docker node ls`                             |
| `docker node inspect <node-id>` | 查看指定节点的详细信息      | `docker node inspect <node-id>`              |
| `docker node update`            | 更新节点的配置              | `docker node update --role worker <node-id>` |
| `docker node rm <node-id>`      | 从 Swarm 集群中移除指定节点 | `docker node rm <node-id>`                   |
| `docker node promote <node-id>` | 将工作节点提升为管理节点    | `docker node promote <node-id>`              |
| `docker node demote <node-id>`  | 将管理节点降级为工作节点    | `docker node demote <node-id>`               |
| `docker node ps <node-id>`      | 列出运行在指定节点上的任务  | `docker node ps <node-id>`                   |

## 服务管理命令

| 命令                                      | 描述                        | 示例                                                                        |
| ----------------------------------------- | --------------------------- | --------------------------------------------------------------------------- |
| `docker service create`                 | 创建一个服务                | `docker service create --name my-web --image nginx --replicas 3 -p 80:80` |
| `docker service ls`                     | 列出 Swarm 集群中的所有服务 | `docker service ls`                                                       |
| `docker service inspect <service-name>` | 查看指定服务的详细信息      | `docker service inspect <service-name>`                                   |
| `docker service update`                 | 更新服务的配置              | `docker service update --image nginx:latest --replicas 5 my-web`          |
| `docker service rm <service-name>`      | 从 Swarm 集群中移除指定服务 | `docker service rm my-web`                                                |
| `docker service scale <service-name>=n` | 扩展服务到指定数量的副本    | `docker service scale my-web=5`                                           |
| `docker service logs <service-name>`    | 查看服务的日志              | `docker service logs my-web`                                              |
| `docker service ps <service-name>`      | 列出服务中的任务            | `docker service ps my-web`                                                |

## 网络管理命令

| 命令                                      | 描述                        | 示例                                                  |
| ----------------------------------------- | --------------------------- | ----------------------------------------------------- |
| `docker network create`                 | 创建一个网络                | `docker network create --driver overlay my-network` |
| `docker network ls`                     | 列出 Swarm 集群中的所有网络 | `docker network ls`                                 |
| `docker network inspect <network-name>` | 查看指定网络的详细信息      | `docker network inspect my-network`                 |
| `docker network rm <network-name>`      | 从 Swarm 集群中移除指定网络 | `docker network rm my-network`                      |
| `docker network prune`                  | 删除所有未使用的网络        | `docker network prune`                              |

## 堆栈管理命令

| 命令                                   | 描述                           | 示例                                                   |
| -------------------------------------- | ------------------------------ | ------------------------------------------------------ |
| `docker stack deploy`                | 部署一个新的堆栈或更新现有堆栈 | `docker stack deploy -c docker-compose.yml my-stack` |
| `docker stack ls`                    | 列出所有堆栈                   | `docker stack ls`                                    |
| `docker stack ps <stack-name>`       | 列出堆栈中的任务               | `docker stack ps my-stack`                           |
| `docker stack rm <stack-name>`       | 移除一个或多个堆栈             | `docker stack rm my-stack`                           |
| `docker stack services <stack-name>` | 列出堆栈中的服务               | `docker stack services my-stack`                     |

## 密钥和配置管理命令

| 命令                                   | 描述                 | 示例                                            |
| -------------------------------------- | -------------------- | ----------------------------------------------- |
| `docker secret create <name> <file>` | 从文件中创建一个密钥 | `docker secret create my_secret ./secret.txt` |
| `docker secret ls`                   | 列出所有密钥         | `docker secret ls`                            |
| `docker secret rm <secret-name>`     | 移除一个密钥         | `docker secret rm my_secret`                  |
| `docker config create <name> <file>` | 从文件中创建一个配置 | `docker config create my_config ./config.txt` |
| `docker config ls`                   | 列出所有配置         | `docker config ls`                            |
| `docker config rm <config-name>`     | 移除一个配置         | `docker config rm my_config`                  |
