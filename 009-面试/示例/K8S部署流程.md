## 1. 准备工作

- **硬件：** 至少两台机器（Master + Node），每台机器至少2核CPU和2GB内存，网络互通。
- **软件：** CentOS 7或Ubuntu 18.04+，Docker，kubeadm，kubelet，kubectl。
- **其他：** 关闭防火墙和SELinux，配置主机名和hosts文件。

## 2. 安装Docker

在所有节点上执行以下命令安装Docker并启动：

Bash

```
# CentOS
yum install -y docker
systemctl start docker
systemctl enable docker

# Ubuntu
apt-get update
apt-get install -y docker.io
systemctl start docker
systemctl enable docker
```

## 3. 安装Kubernetes组件

在所有节点上执行以下命令安装kubeadm、kubelet和kubectl，并启动kubelet：

- **kubeadm：** 负责创建和管理 Kubernetes 集群。
- **kubelet：** 负责运行和监控 Node 节点上的容器和 Pod。
- **kubectl：** 负责与 Kubernetes 集群交互，管理集群中的资源和应用程序。

Bash

```
# CentOS
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
yum install -y kubelet kubeadm kubectl
systemctl start kubelet
systemctl enable kubelet

# Ubuntu
apt-get update
apt-get install -y kubelet kubeadm kubectl
systemctl start kubelet
systemctl enable kubelet
```

## 4. 初始化Master节点

在Master节点上执行以下命令初始化集群，并复制生成的join命令：

Bash

```
kubeadm init --pod-network-cidr=10.244.0.0/16
```

## 5. 配置kubectl

在Master节点上执行以下命令配置kubectl：

Bash

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## 6. 加入Node节点

在Node节点上执行join命令，将其加入到集群中。

## 7. 部署网络插件

部署网络插件（如Flannel），提供Pod之间的网络通信：

Bash

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

## 8. 验证部署

在Master节点上执行以下命令验证部署是否成功：

Bash

```
kubectl get nodes
```

## 核心命令

- `kubeadm init`：初始化Master节点。
- `kubeadm join`：将Node节点加入集群。
- `kubectl`：与Kubernetes集群交互的命令行工具。