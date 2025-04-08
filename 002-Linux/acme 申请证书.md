# acme.sh申请SSL证书

## 步骤

```bash
curl https://get.acme.sh | sh
~/.acme.sh/acme.sh --register-account -m 你的邮箱
~/.acme.sh/acme.sh --issue -d 你的域名 --standalone
```

## 说明

- 安装到主目录，自动设置cron
- 注册账户用于接收证书通知
- 申请证书需临时占用80端口

## 安装证书

```bash
~/.acme.sh/acme.sh --install-cert -d 你的域名 \
--key-file /path/to/key.pem \
--fullchain-file /path/to/fullchain.pem
```

## 提示

- 证书路径: `~/.acme.sh/你的域名/`
- 自动更新: 每天检查并更新
- 多域名: 使用多个 `-d`参数
- DNS验证: 适用于80端口被占用情况

[官方文档](https://github.com/acmesh-official/acme.sh/wiki)
