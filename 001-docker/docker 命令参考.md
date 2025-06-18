## 基本命令
| **命令**           | **功能**                  | **示例**           |
| ---------------- | ----------------------- | ---------------- |
| `docker version` | 显示 Docker 客户端和守护进程的版本信息 | `docker version` |
| `docker info`    | 显示 Docker 系统的详细信息       | `docker info`    |
| `docker login`   | 登录 Docker 仓库            | `docker login`   |
| `docker logout`  | 登出 Docker 仓库            | `docker logout`  |
| `docker help`    | 显示Docker命令帮助信息          | `docker help`    |

## 容器生命周期管理
| **命令**                | **功能**                             | **示例**                                     |
| --------------------- | ---------------------------------- | ------------------------------------------ |
| `docker run`          | 启动一个新的容器并运行命令                      | `docker run -d ubuntu`                     |
| `docker start`        | 启动已停止的容器                           | `docker start container_name`              |
| `docker stop`         | 停止一个或多个容器                          | `docker stop container_name`               |
| `docker restart`      | 重启一个容器                             | `docker restart container_name`            |
| `docker kill`         | 强制停止一个容器                           | `docker kill container_name`               |
| `docker rm`           | 删除一个或多个容器                          | `docker rm container_name`                 |
| `docker pause`        | 暂停容器中的进程                           | `docker pause container_name`              |
| `docker unpause`      | 恢复容器中被暂停的进程                        | `docker unpause container_name`            |
| `docker create`       | 创建一个新容器但不启动                        | `docker create ubuntu`                     |

## 容器操作
| **命令**                | **功能**                             | **示例**                                     |
| --------------------- | ---------------------------------- | ------------------------------------------ |
| `docker ps`           | 列出当前正在运行的容器                        | `docker ps`                                |
| `docker ps -a`        | 列出所有容器（包括已停止的容器）                   | `docker ps -a`                             |
| `docker exec`         | 在运行的容器中执行命令                        | `docker exec -it container_name bash`      |
| `docker exec -it`     | 进入容器的交互式终端                         | `docker exec -it container_name /bin/bash` |
| `docker logs`         | 查看容器的日志                            | `docker logs container_name`               |
| `docker inspect`      | 获取容器或镜像的详细信息                       | `docker inspect container_name`            |
| `docker stats`        | 显示容器的实时资源使用情况                      | `docker stats`                             |
| `docker top`          | 显示容器中运行的进程                         | `docker top container_name`                |
| `docker diff`         | 检查容器文件系统上的更改                       | `docker diff container_name`               |
| `docker port`         | 列出容器的端口映射                          | `docker port container_name`               |
| `docker update`       | 更新容器的配置                            | `docker update --memory 1G container_name`  |
| `docker rename`       | 重命名容器                              | `docker rename old_name new_name`          |
| `docker wait`         | 阻塞直到容器停止，然后打印退出代码                  | `docker wait container_name`               |
| `docker attach`       | 连接到正在运行的容器                         | `docker attach container_name`             |

## 镜像管理
| **命令**                | **功能**                             | **示例**                                     |
| --------------------- | ---------------------------------- | ------------------------------------------ |
| `docker images`       | 列出本地存储的所有镜像                        | `docker images`                            |
| `docker build`        | 使用 Dockerfile 构建镜像                 | `docker build -t my-image .`               |
| `docker pull`         | 从 Docker 仓库拉取镜像                    | `docker pull ubuntu`                       |
| `docker push`         | 将镜像推送到 Docker 仓库                   | `docker push my-image`                     |
| `docker rmi`          | 删除一个或多个镜像                          | `docker rmi my-image`                      |
| `docker tag`          | 为镜像创建一个新的标签                        | `docker tag source_image:tag target_image:tag` |
| `docker save`         | 将镜像保存为tar归档文件                      | `docker save -o image.tar my-image`        |
| `docker load`         | 从tar归档文件加载镜像                       | `docker load -i image.tar`                 |
| `docker history`      | 显示镜像的历史记录                          | `docker history my-image`                  |
| `docker commit`       | 从容器创建一个新镜像                         | `docker commit container_id new_image`     |
| `docker search`       | 在Docker Hub中搜索镜像                   | `docker search ubuntu`                     |
| `docker prune`        | 删除未使用的Docker对象                     | `docker system prune`                      |

## 网络管理
| **命令**                | **功能**                             | **示例**                                     |
| --------------------- | ---------------------------------- | ------------------------------------------ |
| `docker network ls`   | 列出所有 Docker 网络                     | `docker network ls`                        |
| `docker network create` | 创建一个Docker网络                      | `docker network create my-network`         |
| `docker network rm`   | 删除一个或多个网络                          | `docker network rm my-network`             |
| `docker network inspect` | 显示一个或多个网络的详细信息                    | `docker network inspect my-network`        |
| `docker network connect` | 将容器连接到网络                          | `docker network connect my-network container_name` |
| `docker network disconnect` | 将容器与网络断开连接                    | `docker network disconnect my-network container_name` |
| `docker network prune` | 删除所有未使用的网络                        | `docker network prune`                     |

## 存储管理
| **命令**                | **功能**                             | **示例**                                     |
| --------------------- | ---------------------------------- | ------------------------------------------ |
| `docker volume ls`    | 列出所有 Docker 卷                      | `docker volume ls`                         |
| `docker volume create` | 创建一个卷                             | `docker volume create my-volume`           |
| `docker volume rm`    | 删除一个或多个卷                          | `docker volume rm my-volume`               |
| `docker volume inspect` | 显示一个或多个卷的详细信息                     | `docker volume inspect my-volume`          |
| `docker volume prune` | 删除所有未使用的卷                         | `docker volume prune`                      |
| `docker cp`          | 在容器和本地文件系统之间复制文件/文件夹              | `docker cp container_name:/path/file.txt /host/path/` |

## Docker Compose
| **命令**                | **功能**                             | **示例**                                     |
| --------------------- | ---------------------------------- | ------------------------------------------ |
| `docker-compose up`   | 启动多容器应用（从 `docker-compose.yml` 文件） | `docker-compose up`                        |
| `docker-compose down` | 停止并删除由 `docker-compose` 启动的容器、网络等  | `docker-compose down`                      |
| `docker-compose ps`   | 列出由docker-compose启动的容器            | `docker-compose ps`                        |
| `docker-compose logs` | 查看docker-compose服务的日志             | `docker-compose logs service_name`         |
| `docker-compose build` | 构建docker-compose服务                | `docker-compose build`                     |
| `docker-compose restart` | 重启docker-compose服务              | `docker-compose restart service_name`      |
| `docker-compose stop` | 停止docker-compose服务                | `docker-compose stop service_name`         |
| `docker-compose start` | 启动docker-compose服务               | `docker-compose start service_name`        |
| `docker-compose exec` | 在运行的服务容器中执行命令                     | `docker-compose exec service_name bash`    |

## 系统和监控
| **命令**                | **功能**                             | **示例**                                     |
| --------------------- | ---------------------------------- | ------------------------------------------ |
| `docker stats`        | 显示容器的实时资源使用情况                      | `docker stats`                             |
| `docker events`       | 获取服务器的实时事件                         | `docker events`                            |
| `docker system df`    | 显示Docker磁盘使用情况                     | `docker system df`                         |
| `docker system prune` | 删除未使用的数据                           | `docker system prune`                      |
