**1. 安装Samba服务器**

- **Debian/Ubuntu:**
    
    
    ```Bash
    sudo apt update
    sudo apt install samba
    ```
    
- **CentOS/RHEL/Fedora:**
    
    Bash
    
    ```Bash
    sudo yum install samba samba-client
    # 或
    sudo dnf install samba samba-client
    ```
    
- **openSUSE:**
    
    Bash
    
    ```Bash
    sudo zypper install samba
    ```
    

**2. 配置Samba服务器 (smb.conf)**

- **备份配置文件:**
    
    Bash
    
    ```Bash
    sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
    ```
    
- **编辑 `smb.conf`:**
    
    Bash
    
    ```Bash
    sudo nano /etc/samba/smb.conf
    ```
    
- **全局设置 ([global] 部分), 修改或添加:**
    
    Ini, TOML
    
    ```Bash
    [global]
        workgroup = WORKGROUP
        security = user
        encrypt passwords = yes
        smb ports = 139 445
        dns proxy = no
        log file = /var/log/samba/log.%m
        max log size = 1000
    ```
    
    **重要:** 根据网络环境修改 `workgroup` 和 `security`。
    
- **定义共享目录 (添加或修改):**
    
    - **公共共享 (Public Share):**
        
        Ini, TOML
        
        ```Bash
        [public]
            comment = Public Share
            path = /srv/samba/public
            public = yes
            writable = no
            browsable = yes
            guest ok = yes
        ```
        
    - **私有共享 (Private Share):**
        
        Ini, TOML
        
        ```Bash
        [private]
            comment = Private Share
            path = /srv/samba/private
            valid users = @smb_users
            read only = no
            browsable = yes
            guest ok = no
        ```
        
    - **用户家目录共享 ([homes] 部分，取消注释并修改):**
        
        Ini, TOML
        
        ```
        [homes]
            comment = Home Directories
            browseable = no
            read only = no
            create mask = 0700
            directory mask = 0700
            valid users = %S
            guest ok = no
        ```
        
- **创建共享目录:**
    
    Bash
    
    ```
    sudo mkdir -p /srv/samba/public /srv/samba/private
    sudo chmod -R 0777 /srv/samba/public
    sudo chmod -R 0770 /srv/samba/private
    ```
    
- **创建 Samba 用户 (如果需要用户认证):**
    
    Bash
    
    ```
    sudo smbpasswd -a <用户名>
    ```
    
- **创建 Samba 用户组 (如果使用组权限):**
    
    Bash
    
    ```
    sudo groupadd smb_users
    sudo usermod -aG smb_users <用户名>
    ```
    

**3. 测试配置文件**

Bash

```
testparm
```

**4. 重启 Samba 服务**

Bash

```
sudo systemctl restart smbd nmbd
# 或
sudo service smbd restart
sudo service nmbd restart
```

**5. 配置防火墙**

- **firewalld:**
    
    Bash
    
    ```
    sudo firewall-cmd --permanent --add-service=samba
    sudo firewall-cmd --reload
    ```
    
- **ufw:**
    
    Bash
    
    ```
    sudo ufw allow samba
    sudo ufw reload
    ```
    

**6. SELinux (如果启用)**

Bash

```
sudo setsebool -P samba_export_all_ro=1
sudo setsebool -P samba_export_all_rw=1
```

**7. 访问 Samba 共享**

- **Windows:** `\\<Samba服务器IP地址>` 或 `\\<Samba服务器NetBIOS名称>`
- **Linux (文件管理器):** `smb://<Samba服务器IP地址>/<共享名称>`
- **Linux (smbclient):** `smbclient //<Samba服务器IP地址>/<共享名称> -U <用户名>`
- **Linux (mount.cifs):** `sudo mount.cifs //<Samba服务器IP地址>/<共享名称> <本地挂载点> -o user=<用户名>,password=<密码>`