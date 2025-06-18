### 1.快捷创建python venv环境

添加以下alias到.bashrc

>`nano ~/.bashrc`

```shell
alias python='python3'
alias activate='if [ -d "venv" ]; then source venv/bin/activate; else echo "当前目录无Python虚拟环境"; fi'
```

