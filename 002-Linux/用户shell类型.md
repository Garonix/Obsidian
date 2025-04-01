
### 查看用户当前shell

```
echo $SHELL
```

### 查看用户默认shell

```
cat /etc/passwd
```

### 查看系统支持shell

```
cat /etc/shells
```

### 修改用户默认shell

```
chsh
```

### 修改指定用户shell

```
chsh -s /bin/bash username  # 将用户shell改为bash
chsh -s /bin/zsh username   # 将用户shell改为zsh
```

### shell配置文件

- bash: ~/.bashrc, ~/.bash_profile
- zsh: ~/.zshrc
- fish: ~/.config/fish/config.fish
