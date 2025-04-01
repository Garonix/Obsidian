### `usermod -l new_username old_username`

  

- **作用**：修改用户的登录名。
- **示例**：`usermod -l newuser olduser`

### `usermod -c "comment" username`

  

- **作用**：修改用户账户的备注信息。
- **示例**：`usermod -c "This is a test user" testuser`

### `usermod -d home_directory username`

  

- **作用**：修改用户的主目录。
- **示例**：`usermod -d /home/newhome testuser`

### `usermod -g groupname username`

  

- **作用**：修改用户的主组。
- **示例**：`usermod -g newgroup testuser`

### `usermod -G group1,group2 username`

  

- **作用**：修改用户的附加组。
- **示例**：`usermod -G group1,group2 testuser`

### `usermod -a -G groupname username`

  

- **作用**：将用户添加到指定的附加组中，而不删除该用户已有的附加组。
- **示例**：`usermod -a -G newgroup testuser`

### `usermod -s shell_path username`

  

- **作用**：修改用户的默认 shell。
- **示例**：`usermod -s /bin/bash testuser`

### `usermod -e YYYY-MM-DD username`

  

- **作用**：设置用户账户的过期日期。
- **示例**：`usermod -e 2025-12-31 testuser`

### `usermod -f days username`

  

- **作用**：设置用户密码过期后到账户被锁定之间的天数。
- **示例**：`usermod -f 10 testuser`

### `usermod -L username`

  

- **作用**：锁定用户账户，使其无法登录。
- **示例**：`usermod -L testuser`

### `usermod -U username`

  

- **作用**：解锁用户账户。
- **示例**：`usermod -U testuser`

分享

除了以上示例，还有哪些常见的usermod使用场景？

usermod命令是否需要管理员权限才能执行？

如何撤销usermod命令对用户账户的修改？