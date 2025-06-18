1.快速删除\<none\>镜像
```bash
docker rmi $(docker images -f "dangling=true" -q)
```

2.清理docker
```shell
docker system prune -a -f --volumes
```
