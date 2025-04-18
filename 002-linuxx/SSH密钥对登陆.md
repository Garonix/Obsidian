### 1.本地生成密钥对
```bash
ssh-keygen -t rsa -b 2048
```
默认存储到`~/.ssh`目录下
### 2.将公钥发送到服务器
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_server
```
>这里就是通过ssh发送的，注意`路径`、`用户名`和`密码`，必要时添加`-p 端口号`
### 3.SSH命令连接即可
```bash
ssh -p xxx user@remote_server -i /path/to/private/key
```