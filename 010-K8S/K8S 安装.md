# 主从节点公共部分

## 系统准备

### 基本工具安装

```bash
apt install -y sudo nano
```

### 代理设置

若需要代理（临时）：

```bash
export http_proxy=http://192.168.0.1:10808
export https_proxy=http://192.168.0.1:10808
```

### 使用镜像源

若使用镜像源，修改apt源列表：

```bash
nano /etc/apt/sources.list
```

替换为清华镜像源：

```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
# deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
```

### 更新系统和安装必要软件包

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt autoremove
sudo apt autoclean
sudo apt install -y apt-transport-https ca-certificates vim git curl gpg
```
> 注意：如果更新了内核，需要重启

### 禁用交换分区

```bash
sudo swapoff -a
```

```bash
nano /etc/fstab    # 注释掉swap相关的行
```
## 配置内核模块及系统参数

### 加载必要的内核模块

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

### 配置网络参数

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
```

## 安装容器运行时

### 安装Docker

会默认一并安装containerd（推荐）

```bash
curl -fsSL get.docker.com -o get-docker.sh && sudo sh get-docker.sh --mirror Aliyun

docker -v  # 查看docker版本
systemctl enable --now docker  # 设置开机自动启动
```

### 配置Docker代理

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

### 安装Docker CRI (可选，不推荐)

^87fcd7

```bash
# 克隆 Mirantis 的 cri-dockerd 仓库，--depth 1 表示只克隆最新的一次提交，减少克隆的数据量
git clone https://github.com/Mirantis/cri-dockerd.git --depth 1

# 获取 cri-dockerd 的最新版本号
VERSION=$(curl -s "https://api.github.com/repos/Mirantis/cri-dockerd/releases/latest" | grep '"tag_name":' | cut -d '"' -f 4 | sed 's/v//')

# 下载指定版本的 cri-dockerd 压缩包
curl -LO https://github.com/Mirantis/cri-dockerd/releases/download/v$VERSION/cri-dockerd-$VERSION.amd64.tgz

# 解压下载的 cri-dockerd 压缩包
tar -zxvf cri-dockerd-$VERSION.amd64.tgz

# 进入解压后的 cri-dockerd 目录
cd cri-dockerd

# 将 cri-dockerd 可执行文件安装到 /usr/local/bin 目录，并设置所有者为 root，所属组为 root，权限为 0755
install -o root -g root -m 0755 cri-dockerd /usr/local/bin/cri-dockerd

# 将 cri-dockerd 相关的 systemd 配置文件安装到 /etc/systemd/system 目录
install packaging/systemd/* /etc/systemd/system

# 修改 /etc/systemd/system/cri-docker.service 文件中的 cri-dockerd 可执行文件路径
# 将 /usr/bin/cri-dockerd 替换为 /usr/local/bin/cri-dockerd
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service

# 重新加载 systemd 守护进程，使新的配置文件生效
systemctl daemon-reload

# 启用并立即启动 cri-docker.socket 服务
systemctl enable --now cri-docker.socket

# 启用并立即启动 cri-docker.service 服务
systemctl enable --now cri-docker.service

# 修改 pause 配置：
# 使用 nano 编辑器打开 /etc/systemd/system/cri-docker.service 文件
nano /etc/systemd/system/cri-docker.service

# 修改 ExecStart 行，指定 cri-dockerd 的启动参数
# --container-runtime-endpoint fd:// 表示使用文件描述符作为容器运行时的端点
# --network-plugin=cni 表示使用 CNI 作为网络插件
# --pod-infra-container-image=registry.aliyuncs.com/google_containers/pause:3.10 表示使用阿里云镜像仓库的 pause 镜像版本 3.10
ExecStart=/usr/local/bin/cri-dockerd --container-runtime-endpoint fd:// --network-plugin=cni --pod-infra-container-image=registry.aliyuncs.com/google_containers/pause:3.10
```

## 配置Containerd

### 生成默认配置文件

```bash
sudo containerd config default > /etc/containerd/config.toml
```

### 修改配置

^af8d50

添加注释（不一定需要，但是注意）
```
# disabled_plugins = ["cri"]
```

修改`/etc/containerd/config.toml`中的sandbox_image： ^7312b4

```toml
sandbox_image = "registry.aliyuncs.com/google_containers/pause:3.10"
```

### 重启containerd

```bash
systemctl restart containerd
```

## 安装Kubernetes组件

### 安装kubectl、kubeadm和kubelet

#### 方法一：curl下载安装（不推荐，需手动配置服务）
curl下载安装
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubeadm"
sudo install -o root -g root -m 0755 kubeadm /usr/local/bin/kubeadm

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubelet"
sudo install -o root -g root -m 0755 kubelet /usr/local/bin/kubelet
```

手动配置kubelet为系统服务
```bash
sudo tee /etc/systemd/system/kubelet.service <<EOF
[Unit]
Description=Kubernetes Kubelet
Documentation=https://kubernetes.io/docs/home/
After=network.target
Wants=containerd.service

[Service]
ExecStart=/usr/local/bin/kubelet
Restart=always
StartLimitInterval=0
RestartSec=10

[Install]
WantedBy=multi-user.target
EOF
```

创建一个 `kubelet` 的环境变量文件，用于传递一些额外的配置参数给 `kubelet`
```bash
sudo tee /etc/sysconfig/kubelet <<EOF
KUBELET_EXTRA_ARGS=
EOF
```

```bash
sudo systemctl daemon-reload
sudo systemctl start kubelet 
sudo systemctl enable kubelet
```
#### 方法二：系统包管理器安装（推荐）

```bash
# 如果 `/etc/apt/keyrings` 目录不存在，则应在 curl 命令之前创建它
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# 此操作会覆盖 /etc/apt/sources.list.d/kubernetes.list 中现存的所有配置
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubectl kubeadm
sudo apt-mark hold kubelet kubeadm kubectl
```

## 主节点部分

### 使用阿里云镜像源拉取必要的镜像

```bash
kubeadm config images pull --image-repository registry.aliyuncs.com/google_containers
```

### 生成默认配置

```bash
kubeadm config print init-defaults > kubeadm-config.yaml
```

### 修改配置文件

修改`kubeadm-config.yaml`配置文件：

```yaml
localAPIEndpoint:
  advertiseAddress: 192.168.0.10  # 替换为实际的控制平面节点 IP 地址
# ...existing code...
imageRepository: registry.aliyuncs.com/google_containers
# ...existing code...
networking:
  dnsDomain: cluster.local
  podSubnet: 10.10.0.0/16
  serviceSubnet: 10.96.0.0/12
  
# 修改版本号

# ...existing code...
nodeRegistration:
  name: master
```

### 初始化集群

```bash
kubeadm init --config=kubeadm-config.yaml --upload-certs
```

### 配置kubectl

普通用户配置：
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

root用户配置：
```bash
export KUBECONFIG=/etc/kubernetes/admin.conf
```

## 安装网络插件Calico

```bash
# 部署tigera-operator
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.29.3/manifests/tigera-operator.yaml

# 下载calico配置
curl https://raw.githubusercontent.com/projectcalico/calico/v3.29.3/manifests/custom-resources.yaml -O

# 修改配置
nano custom-resources.yaml
# ...existing code...
cidr: 10.10.0.0/16 # 修改为podSubnet: 10.10.0.0/16
# ...existing code...

# 部署calico
kubectl create -f custom-resources.yaml
```


## 从节点部分
```bash
# 默认--cri-socket为containerd，按需指定cri-dockerd
kubeadm join 192.168.0.10:6443 --token abcdef.0123456789abcdef --discovery-token-ca-cert-hash sha256:e05a74ce5e4a4e50561f4221545a085e48b6245a491d9afba8eab6abedece68a --cri-socket=unix:///var/run/cri-dockerd.sock
```


# 遇到的一些问题记录
Q：卡在apiserver健康检查
A：可能与pause镜像版本有关，需要在criSocket中配置，[containerd](#^7312b4)，[cri-dcokerd](#^87fcd7)

Q：镜像拉取时查看events，看到还是使用的containerd作为cni
A：可能与docker镜像源配置有关，删除docker镜像源，重启docker
```bash
nano /etc/docker/daemon.json   # 删除其中镜像源配置
systemctl restart docker       # 重启docker
```

Q：8080端口无法访问
A：没有指定配置文件
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Q：使用containerd作为cni，K8S核心组件反复重启
A：可能与containerd版本之类有关，暂时不清楚，使用cri-dockerd作为替代

# 参考
[Kubernetes简介 | MagicGopher Blog](https://magicgopher.cn/docs/zh/DevOps/02-Kubernetes/01-Kubernetes%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/01-Kubernetes-%E7%AE%80%E4%BB%8B.html)