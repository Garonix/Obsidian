### 1. 执行 Ad - Hoc 命令

Ad - Hoc 命令是一种一次性执行的简单命令，适合快速执行单个任务。

#### 基本语法

bash

```bash
ansible <主机组名> -m <模块名> -a "<模块参数>"
```

  

- `<主机组名>`：在主机清单文件（如 `/etc/ansible/hosts`）中定义的主机组名称。
- `<模块名>`：Ansible 提供了众多模块，用于执行不同的任务，例如 `ping`、`command`、`copy` 等。
- `<模块参数>`：根据所选模块的要求提供相应的参数。

#### 示例

- **测试主机连通性**

  

bash

```bash
ansible all -m ping
```

  

此命令使用 `ping` 模块测试所有主机（`all` 表示主机清单中的所有主机）的连通性。

  

- **在远程主机上执行命令**

  

bash

```bash
ansible slaves -m command -a "ls -l"
```

  

该命令使用 `command` 模块在 `slaves` 主机组的所有主机上执行 `ls -l` 命令。

### 2. 使用 Playbook

Playbook 是 Ansible 的核心功能，它使用 YAML 格式编写，用于定义一系列任务，可实现复杂的自动化操作。

#### 基本结构

一个简单的 Playbook 示例如下：

  

yaml

```yaml
---
- name: Install and start Nginx
  hosts: slaves
  become: true
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
    - name: Start Nginx service
      service:
        name: nginx
        state: started
```

  

- `name`：Playbook 的名称，用于描述该 Playbook 的功能。
- `hosts`：指定要执行任务的主机组。
- `become`：若设置为 `true`，表示以特权用户（如 `root`）身份执行任务。
- `tasks`：任务列表，每个任务都有一个名称和对应的模块及参数。

#### 执行 Playbook

bash

```bash
ansible-playbook <playbook文件名>.yml
```

  

例如，执行上述示例的 Playbook：

  

bash

```bash
ansible-playbook install_nginx.yml
```

### 3. 变量的使用

在 Ansible 中，变量可用于存储和传递数据，使 Playbook 更加灵活。

#### 定义变量

在 Playbook 中定义变量：

  

yaml

```yaml
---
- name: Use variables
  hosts: slaves
  vars:
    package_name: nginx
  tasks:
    - name: Install package
      apt:
        name: "{{ package_name }}"
        state: present
```

  

在上述示例中，`package_name` 是一个变量，通过 `{{ package_name }}` 语法引用该变量。

#### 外部变量

也可以使用外部变量文件来存储变量。创建一个变量文件 `vars.yml`：

  

yaml

```yaml
package_name: nginx
```

  

在 Playbook 中引用该变量文件：

  

yaml

```yaml
---
- name: Use external variables
  hosts: slaves
  vars_files:
    - vars.yml
  tasks:
    - name: Install package
      apt:
        name: "{{ package_name }}"
        state: present
```

### 4. 角色的使用

角色是 Ansible 中组织 Playbook 的一种方式，可将相关的任务、变量、模板等组织在一起，提高代码的复用性和可维护性。

#### 创建角色目录结构

bash

```bash
ansible-galaxy init myrole
```

  

该命令会创建一个名为 `myrole` 的角色目录，包含以下子目录：

  

plaintext

```plaintext
myrole/
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
└── vars
    └── main.yml
```

#### 在 Playbook 中使用角色

yaml

```yaml
---
- name: Use role
  hosts: slaves
  roles:
    - myrole
```

### 5. 标签的使用

标签可用于选择性地执行 Playbook 中的任务。

#### 为任务添加标签

yaml

```yaml
---
- name: Use tags
  hosts: slaves
  tasks:
    - name: Install package
      apt:
        name: nginx
        state: present
      tags:
        - install
    - name: Start service
      service:
        name: nginx
        state: started
      tags:
        - start
```

  

#### 选择性执行任务

bash

```bash
ansible-playbook <playbook文件名>.yml --tags "install"
```

  

  

该命令只会执行带有 `install` 标签的任务。

分享

如何创建Ansible的配置文件和主机清单文件？

如何使用Ansible的Playbook实现复杂的自动化操作？

Ansible有哪些常用模块及其作用？