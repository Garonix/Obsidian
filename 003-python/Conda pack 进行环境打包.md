> 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/540615230)

conda 常用来执行虚拟环境配置和包管理工作，有时候需要将本地的环境一直到新的离线的机器上，可以使用 [conda-pack](https://conda.github.io/conda-pack/index.html) 进行 conda 环境打包和分发。

### 安装

conda 安装

```shell
conda install conda-pack
# conda install -c conda-forge conda-pack
```

pip 安装

```shell
pip install conda-pack
```

### 使用教程

构建环境的操作系统必须与目标的操作系统匹配。这意味着在 Windows 上构建的环境不能重新定位到 Linux。

**命令行**

完整的 CLI 文档[在这里](https://conda.github.io/conda-pack/cli.html)

一个常见的用例是在一台机器上打包一个环境，以分发给可能未安装 conda/python 的其他机器。

1.  在源计算机上（根据需求三选一）

```shell
# 把虚拟环境 my_env 打包为 my_env.tar.gz
conda pack -n my_env

# -o 参数指定打包路径和名称，把虚拟环境 my_env 打包为 out_name.tar.gz
conda pack -n my_env -o out_name.tar.gz

# 把某个特定路径的虚拟环境打包为 my_env.tar.gz
conda pack -p /explicit/path/to/my_env

```

2. 在目标计算机上

> 前缀（prefixstr）：本文指到某一 conda 环境的路径

**linux**

```shell
# 创建目录 `my_env`，并将环境解压至该目录
mkdir -p my_env
tar -xzf my_env.tar.gz -C my_env

# 使用python而不激活或修复前缀。
# 大多数 python 库可以正常工作，但需要处理前缀的部分将失败。
./my_env/bin/python

# 激活环境，同时这步操作会将路径 `my_env/bin` 添加到环境变量 path
source my_env/bin/activate

# 在环境中运行python
(my_env) $ python

# 从激活环境中清除前缀。
# 请注意，也可以在不激活环境的情况下运行此命令
# 只要机器上已经安装了某个版本的python。
(my_env) $ conda-unpack

# 此时，环境与您在此路径直接使用 conda 安装的环境完全相同。
# 所有脚本都应该工作正常。
(my_env) $ ipython --version

# 停用环境以将其从环境变量 path 中删除
(my_env) $ source my_env/bin/deactivate

```

**windows**

新建 `my_env` 文件夹，将打包的 my_env.tar.gz 文件解压到该文件夹中。

使用 cmd 打开路径 `my_env` 所在路径

```powershell
# 进入项目路径
cd C:\my_env

# 激活环境 
.\Scripts\activate.bat

# 从激活环境中清除前缀。
.\Scripts\conda-unpack.exe

# 退出环境
.\Scripts\deactivate.bat

```

**Api 模式**

`conda-pack`还提供了一个 Python API，其完整文档可以[在这里](https://conda.github.io/conda-pack/api.html)找到。

```Python
import conda_pack

# 把虚拟环境 my_env 打包为 my_env.tar.gz
conda_pack.pack()

# -o 参数指定打包路径和名称，把虚拟环境 my_env 打包为 out_name.tar.gz
conda_pack.pack()

# 把某个特定路径的虚拟环境打包为 my_env.tar.gz
conda_pack.pack(prefix="/explicit/path/to/my_env")

```

> [Conda-Pack — conda-pack 0.7.0 documentation](https://conda.github.io/conda-pack/)  
> [用 conda-pack 制作离线可移植 python 环境 | mlstars (shiratori3.github.io)](https://shiratori3.github.io/2021/%E7%94%A8conda-pack%E5%88%B6%E4%BD%9C%E7%A6%BB%E7%BA%BF%E5%8F%AF%E7%A7%BB%E6%A4%8Dpython%E7%8E%AF%E5%A2%83/)