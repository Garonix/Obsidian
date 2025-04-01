## 一、准备工作  
<ul>
	<li>一台服务器</li>
	<li>docker compose</li>
	<li><b>非必须</b>：nginx proxy manager
	<li><b>非必须</b>：一个域名</li>
</ul>

## 二、安装alist  
### 1.准备alist工作区  
```bash
# 请按照自己的需求进行调整
mkdir -p /home/docker/alist/    #创建一个名为 alist 的目录，并将其路径设置为 /home/docker/alist
cd /home/docker/alist/ # 进入到 /home/docker/alist 目录中，即刚才创建的 alist 文件夹
touch docker-compose.yaml # 在当前目录下创建一个名为 docker-compose.yaml 的空文件
```

### 2.用docker-compose进行安装  
使用外部编辑器编辑`docker-compose.yaml`文件  
同样，按需进行调整
```yaml
version: '3.3' # compose格式版本号

services: # 下面是 services 列表，定义了一个名为 alist 的服务
  alist:
    image: 'xhofe/alist:latest' # 使用 xhofe/alist:latest 镜像作为容器运行环境
    restart: unless-stopped # 容器退出时会自动重启（除非手动停止）
    volumes: # 挂载数据卷
        - './data:/opt/alist/data' # 将主机目录下的 ./data 目录挂载到容器内的 /opt/alist/data 目录下
    ports: # 暴露端口
        - '5244:5244' # 将主机上的 5244 端口映射到容器内的 5244 端口
    environment: # 设置环境变量
        - PUID=0 # 指定容器运行用户的 UID
        - PGID=0 # 指定容器运行用户的 GID
        - UMASK=022 # 指定 umask 值
    container_name: alist # 自定义容器名称

```  
编辑完成后记得`保存`  
###  3.创建容器并运行  
```shell
docker-compose up -d  # 默认后台运行
```
程序会自动拉取镜像并创建容器，耐心等待命令执行完毕  
现在应该可以使用http://ip:5244访问alist了  
添加网盘请参见[官方文档](https://alist.nn.ci/zh/guide/#support-storage)
## 附：使用npm反代alist  
### 1. 添加域名记录  
请以自己的域名托管的网站为准，这里以`cloudflare`为例  
访问[cloudflare](https://dash.cloudflare.com/)并登录  
点击自己托管的域名  
点击左侧边栏的`DNS`   

![1681651079426.png](https://img.zeges.top/FH5pkA.png)  

### 2.添加反代  
访问`nginx proxy manager`并登录
进入`Proxy Hosts`  
点击右上角的`Add Proxy Host`  

![1681650532284.png](https://img.zeges.top/mvQ6Er.png)  
![1681650690570.png](https://img.zeges.top/kGgjWm.png)  
![1681650636482.png](https://img.zeges.top/bR757t.png)  

现在应该能用域名记录访问alist了




