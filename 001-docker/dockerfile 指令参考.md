
### 基础配置类指令

| 指令         | 说明                                         | 用法示例                                                                             |
| ---------- | ------------------------------------------ | -------------------------------------------------------------------------------- |
| **FROM**   | 指定基础镜像，用于后续的指令构建。                          | `FROM ubuntu:20.04` `<br>` `FROM node:14-alpine`                                 |
| **LABEL**  | 添加镜像的元数据，使用键值对的形式。                         | `LABEL version="1.0" description="Web Application" maintainer="dev@example.com"` |
| **ENV**    | 在容器内部设置环境变量。                               | `ENV NODE_ENV production` `<br>` `ENV PATH=/usr/local/bin:$PATH`                 |
| **ARG**    | 定义在构建过程中传递给构建器的变量，可使用 "docker build" 命令设置。 | `ARG VERSION=latest` `<br>` `ARG BUILD_DATE`                                     |

### 执行命令类指令

| 指令                 | 说明                                                    | 用法示例                                                                       |
| -------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **RUN**        | 在构建过程中在镜像中执行命令。                          | `RUN apt-get update && apt-get install -y curl` `<br>` `RUN npm install` |
| **CMD**        | 指定容器创建时的默认命令。（可以被覆盖）                | `CMD ["node", "app.js"]` `<br>` `CMD ["nginx", "-g", "daemon off;"]`     |
| **ENTRYPOINT** | 设置容器创建时的主要命令。（不可被覆盖）                | `ENTRYPOINT ["java", "-jar"]` `<br>` `ENTRYPOINT ["python", "app.py"]`   |
| **SHELL**      | 覆盖Docker中默认的shell，用于RUN、CMD和ENTRYPOINT指令。 | `SHELL ["/bin/bash", "-c"]` `<br>` `SHELL ["powershell", "-command"]`    |

### 文件操作类指令

| 指令              | 说明                                | 用法示例                                                                     |
| ----------------- | ----------------------------------- | ---------------------------------------------------------------------------- |
| **ADD**     | 将文件、目录或远程URL复制到镜像中。 | `ADD app.tar.gz /opt/` `<br>` `ADD https://example.com/file.txt /app/` |
| **COPY**    | 将文件或目录复制到镜像中。          | `COPY package.json /app/` `<br>` `COPY . /workspace`                   |
| **WORKDIR** | 设置后续指令的工作目录。            | `WORKDIR /app` `<br>` `WORKDIR /usr/src/app`                           |
| **VOLUME**  | 为容器创建挂载点或声明卷。          | `VOLUME /data` `<br>` `VOLUME ["/var/log", "/var/db"]`                 |

### 网络与安全类指令

| 指令             | 说明                 | 用法示例                                          |
| -------------- | ------------------ | --------------------------------------------- |
| **EXPOSE**     | 声明容器运行时监听的特定网络端口。  | `EXPOSE 80` `<br>` `EXPOSE 8080/tcp 8081/udp` |
| **USER**       | 指定后续指令的用户上下文。      | `USER nginx` `<br>` `USER node:node`          |
| **STOPSIGNAL** | 设置发送给容器以退出的系统调用信号。 | `STOPSIGNAL SIGTERM` `<br>` `STOPSIGNAL 9`    |

### 高级指令

| 指令              | 说明                        | 用法示例                                                                                                          |
| --------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **ONBUILD**     | 当该镜像被用作另一个构建过程的基础时，添加触发器。 | `ONBUILD RUN npm install` `<br>` `ONBUILD COPY . /app`                                                        |
| **HEALTHCHECK** | 定义周期性检查容器健康状态的命令。         | `HEALTHCHECK --interval=5m CMD curl -f http://localhost/                                                      |
| **MULTI-STAGE** | 多阶段构建，用于优化镜像大小。           | `FROM node:14 AS build` `<br>` `FROM nginx:alpine` `<br>` `COPY --from=build /app/dist /usr/share/nginx/html` |

