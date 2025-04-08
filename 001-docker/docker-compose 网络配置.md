
## 一、基本概念

Docker Compose 默认为应用创建网络，使各容器可通过服务名作为hostname相互访问。应用网络名称基于工程名，即docker-compose.yml所在目录名。可通过 `--project-name`标识或 `COMPOSE_PROJECT_NAME`环境变量修改工程名。

**示例：**

假设应用位于 `myapp`目录，`docker-compose.yml`内容如下：

```yaml
version: '2'
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres
```

执行 `docker-compose up`时：

1. 创建名为 `myapp_default`的网络
2. 创建web服务容器并加入网络，hostname为"web"
3. 创建db服务容器并加入网络，hostname为"db"

各容器可通过服务名互访，如web服务可通过 `postgres://db:5432`访问数据库。

## 二、更新容器

当服务配置变更时，使用 `docker-compose up`更新配置。Compose会替换旧容器，新容器IP可能变更但名称保持不变。旧连接会关闭，服务会自动连接到新容器。

## 三、Links

通过 `links`可为服务定义别名：

```yaml
version: '2'
services:
  web:
    build: .
    links:
      - "db:database"
  db:
    image: postgres
```

这样web服务可同时使用 `db`和 `database`作为hostname访问数据库。

## 四、自定义网络

当默认网络配置不满足需求时，可使用 `networks`自定义网络拓扑：

```yaml
version: '2'
services:
  proxy:
    build: ./proxy
    networks:
      - front
  app:
    build: ./app
    networks:
      - front
      - back
  db:
    image: postgres
    networks:
      - back
networks:
  front:
    driver: custom-driver-1
  back:
    driver: custom-driver-2
    driver_opts:
      foo: "1"
      bar: "2"
```

在此配置中，proxy与db服务网络隔离，app服务可与两者通信，实现了网络分段。

## 五、配置默认网络

可为默认网络指定自定义配置：

```yaml
version: '2'
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres
networks:
  default:
    driver: custom-driver-1
```

## 六、使用已存在网络

若需加入已存在的网络而非创建新网络，可使用 `external`选项：

```yaml
networks:
  default:
    external:
      name: my-pre-existing-network
```

>[原文地址](https://blog.csdn.net/chen_jianjian/article/details/106674040?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-106674040-blog-113415659.pc_relevant_aa2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-106674040-blog-113415659.pc_relevant_aa2&utm_relevant_index=4)