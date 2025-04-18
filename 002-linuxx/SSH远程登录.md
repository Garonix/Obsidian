## 使用 SSH 密钥登录服务器

使用 SSH 密钥登录服务器主要分为以下几个步骤：

1.  **生成 SSH 密钥对 (如果还没有)：**
    * 在你的本地计算机上打开终端或命令提示符。
    * 运行以下命令：
        ```bash
        ssh-keygen -t rsa -b 4096
        ```
        * `-t rsa` 指定密钥类型为 RSA。
        * `-b 4096` 指定密钥长度为 4096 位（推荐）。
    * 系统会提示你保存密钥的位置和设置密码（passphrase）。你可以直接回车接受默认设置，或者自定义。建议设置一个密码来增加安全性。
    * 生成成功后，你会得到两个文件：
        * **私钥 (通常是 `id_rsa`):** 你需要妥善保管这个文件，**不要泄露给任何人**。
        * **公钥 (通常是 `id_rsa.pub`):** 这个文件可以安全地共享。

2.  **将公钥复制到服务器：**
    * 有几种方法可以将公钥复制到服务器。最简单的方法是使用 `ssh-copy-id` 命令（如果你的本地和服务器都安装了 SSH）：
        ```bash
        ssh-copy-id username@your_server_ip
        ```
        * 将 `username` 替换为你在服务器上的用户名。
        * 将 `your_server_ip` 替换为服务器的 IP 地址或域名。
    * 如果这是你第一次连接该服务器，系统会提示你确认连接，输入 `yes`。
    * 然后，系统会提示你输入服务器用户的密码。输入密码后，`ssh-copy-id` 会自动将你的公钥添加到服务器上的 `~/.ssh/authorized_keys` 文件中。
    * **如果 `ssh-copy-id` 不可用，你可以手动操作：**
        * 将你的公钥文件 (`id_rsa.pub`) 的内容复制到剪贴板。可以使用 `cat` 命令查看内容：
            ```bash
            cat ~/.ssh/id_rsa.pub
            ```
        * 通过 SSH 连接到服务器（使用密码登录）：
            ```bash
            ssh username@your_server_ip
            ```
        * 在服务器上创建 `.ssh` 目录（如果不存在）：
            ```bash
            mkdir -p ~/.ssh
            chmod 700 ~/.ssh
            ```
        * 编辑或创建 `authorized_keys` 文件：
            ```bash
            nano ~/.ssh/authorized_keys
            ```
            或者
            ```bash
            vim ~/.ssh/authorized_keys
            ```
        * 将你复制的公钥内容粘贴到 `authorized_keys` 文件中。确保每行只有一个公钥。
        * 保存并关闭文件。
        * 设置 `authorized_keys` 文件的权限：
            ```bash
            chmod 600 ~/.ssh/authorized_keys
            ```

3.  **使用 SSH 密钥登录服务器：**
    * 在你的本地计算机上打开终端或命令提示符。
    * 运行以下命令：
        ```bash
        ssh username@your_server_ip
        ```
        * 将 `username` 替换为你在服务器上的用户名。
        * 将 `your_server_ip` 替换为服务器的 IP 地址或域名。
    * 如果你的私钥没有设置密码，你将直接登录到服务器。
    * 如果你的私钥设置了密码，系统会提示你输入密码。输入正确的密码后，你将登录到服务器。

**注意事项：**

* 确保你的私钥文件（例如 `id_rsa`）的权限设置为只有你本人可读（通常是 `600`）。可以使用 `chmod 600 ~/.ssh/id_rsa` 命令设置。
* 服务器上的 `.ssh` 目录的权限通常设置为 `700`，`authorized_keys` 文件的权限通常设置为 `600`。
* 为了安全起见，建议禁用服务器的密码登录，只允许使用 SSH 密钥登录。这需要在服务器的 SSH 配置文件 (`/etc/ssh/sshd_config`) 中进行设置。找到 `PasswordAuthentication` 这一行，将其值改为 `no`，然后重启 SSH 服务。