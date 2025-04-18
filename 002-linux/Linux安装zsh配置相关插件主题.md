## ZSH安装配置指南

### 检查Shell环境

* 查看系统当前使用的shell

```bash
echo $SHELL
```

* 查看当前平台支持的shell

```bash
cat /etc/shells
```

### 安装配置ZSH

* 安装 `zsh`

```bash
yum install -y zsh
```

* 启用 `zsh`

```bash
chsh -s /bin/zsh
```

> 一般内置的root用户执行会失败，无法更改shell，可执行 `/bin/zsh`命令生效或修改 `/etc/passwd`文件（root用户和当前用户都要修改）

* 安装 `oh-my-zsh`

```bash
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

### 安装插件

* 安装 `zsh-autosuggestions`插件

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

* 安装 `zsh-syntax-highlighting`插件

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

* 在 `zsh`中启用插件

```bash
nano ~/.zshrc
```

编辑 `plugins`部分，修改为：

```bash
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```

### 安装主题

* 安装 `Powerlevel10k`主题

```bash
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

* 在 `zsh`中启用主题

```bash
nano ~/.zshrc
```

编辑 `ZSH_THEME`这一项：

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```
