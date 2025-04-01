Ansible 是一种自动化工具，支持主从（控制节点和被管理节点）架构。下面为你详细介绍在一主二从环境下安装 Ansible 的步骤。

### 前提条件

- 所有节点（一个主节点和两个从节点）均需安装 Linux 系统，并且能够通过 SSH 进行相互通信。
- 主节点和从节点的系统账户需具备`sudo`权限。

### 安装步骤

#### 1. 在主节点安装 Ansible

不同的 Linux 发行版，安装 Ansible 的方式有所不同，以下是常见发行版的安装方法。

  

**基于 Debian 或 Ubuntu 的系统**：

  

bash

```bash
sudo apt update
sudo apt install ansible -y
```

  

**基于 Red Hat 或 CentOS 的系统**：

  

bash

```bash
sudo yum install epel-release -y
sudo yum install ansible -y
```

#### 2. 配置 SSH 密钥认证

为了让主节点能无密码访问从节点，需要在主节点生成 SSH 密钥，并将公钥复制到从节点。

  

**在主节点生成 SSH 密钥**：

  

bash

```bash
ssh-keygen
```

  

按提示操作，一路回车即可。

  

**将公钥复制到从节点**：

  

bash

```bash
ssh-copy-id user@slave1_ip
ssh-copy-id user@slave2_ip
```

  

这里的`user`是从节点的用户名，`slave1_ip`和`slave2_ip`分别是两个从节点的 IP 地址。

#### 3. 配置 Ansible 主机清单

自行创建`/etc/ansible/ansible.cfg`，内容如下：
```ini
[defaults]
# 主机清单文件路径
inventory = /etc/ansible/hosts
# 远程用户
remote_user = root
# 并发处理的主机数量
forks = 5
```

```bash
cat << EOF > /etc/ansible/ansible.cfg
[defaults]
# 主机清单文件路径
inventory = /etc/ansible/hosts
# 远程用户
remote_user = root
# 并发处理的主机数量
forks = 5
EOF
```

自行创建 Ansible 的主机清单文件`/etc/ansible/hosts`，添加从节点信息。

```ini
[nodes]
node1 ansible_host=node1
node2 ansible_host=node2

[nodes:vars]
ansible_user=user
```

```bash
cat << EOF > /etc/ansible/hosts
[nodes]
node1 ansible_host=192.168.0.11
node2 ansible_host=192.168.0.12

[nodes:vars]
ansible_user=user
EOF
```
  

其中，`slave1`和`slave2`是自定义的主机名，`ansible_host`是从节点的实际 IP 地址，`ansible_user`是从节点的用户名。

#### 4. 测试 Ansible 连接

在主节点上运行以下命令，测试与从节点的连接：

  

bash

```bash
ansible slaves -m ping
```

  

  

若所有从节点都返回`SUCCESS`，则表明连接成功。