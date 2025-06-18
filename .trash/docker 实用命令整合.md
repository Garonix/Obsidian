1.快速删除<none>镜像
`docker rmi $(docker images -f "dangling=true" -q)`