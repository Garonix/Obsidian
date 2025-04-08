| 名称      | 方法                         | 功能                             |
| ------- | -------------------------- | ------------------------------ |
| 文件和目录操作 | `os.rename(src, dst)`      | 重命名文件或目录，从`src`改为`dst`         |
|         | `os.remove(path)`          | 删除指定路径的文件                      |
|         | `os.rmdir(path)`           | 删除指定路径的空目录                     |
|         | `os.mkdir(path)`           | 创建一个目录                         |
|         | `os.makedirs(path)`        | 创建多层目录，如果目录不存在则递归创建            |
|         | `os.listdir(path)`         | 返回指定路径下的文件和目录列表                |
|         | `os.path.exists(path)`     | 判断指定路径是否存在                     |
| 路径操作    | `os.path.join(*paths)`     | 将多个路径合并成一个路径，自动处理分隔符（如`/`或`\`） |
|         | `os.path.abspath(path)`    | 返回指定路径的绝对路径                    |
|         | `os.path.basename(path)`   | 返回路径的最后一个文件名或目录名               |
|         | `os.path.dirname(path)`    | 返回路径的目录部分                      |
| 环境变量    | `os.getenv(key)`           | 获取环境变量的值，若不存在则返回`None`         |
|         | `os.environ`               | 一个字典，包含当前进程的环境变量               |
| 进程管理    | `os.system(command)`       | 执行系统命令，并返回命令的退出状态码             |
|         | `os.spawn()`               | 启动新进程，提供更灵活的进程创建方式（较少使用）       |
|         | `os.getpid()`              | 获取当前进程的ID                      |
|         | `os.getppid()`             | 获取当前进程的父进程ID                   |
| 系统信息    | `os.name`                  | 返回当前操作系统的名称（如`posix`、`nt`等）    |
|         | `os.uname()`               | 获取操作系统的详细信息（仅在类Unix系统上可用）      |
| 文件权限和属性 | `os.chmod(path, mode)`     | 修改文件或目录的权限                     |
|         | `os.chown(path, uid, gid)` | 更改文件的所有者和组                     |
