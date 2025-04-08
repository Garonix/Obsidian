## sed 命令

`sed` 是利用脚本来处理文本文件的工具，可依照脚本指令来处理、编辑文本文件。主要用于自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序等。

### 语法

```bash
sed [-hnV][-e<script>][-f<script文件>][文本文件]
```

**参数说明**：

- `-e<script>`或 `--expression=<script>` - 以选项中指定的script处理输入文本文件
- `-f<script文件>`或 `--file=<script文件>` - 以选项中指定的script文件处理输入文本文件
- `-h`或 `--help` - 显示帮助
- `-n`或 `--quiet`或 `--silent` - 仅显示script处理后的结果
- `-V`或 `--version` - 显示版本信息

**动作说明**：

- `a` - 新增，在当前行下方添加文本
- `c` - 取代，替换指定行的内容
- `d` - 删除行
- `i` - 插入，在当前行上方添加文本
- `p` - 打印，通常与参数 `-n` 一起运行
- `s` - 取代，可以直接进行替换工作，常与正则表达式搭配

### 实例

先创建一个测试文件：

```bash
$ cat testfile
HELLO LINUX!  
Linux is a free unix-type opterating system.  
This is a linux testfile!  
Linux test 
Google
Taobao
Runoob
Tesetfile
Wiki
```

#### 添加行

在第四行后添加一行：

```bash
$ sed -e '4a\newLine' testfile 
HELLO LINUX!  
Linux is a free unix-type opterating system.  
This is a linux testfile!  
Linux test 
newLine
Google
Taobao
Runoob
Tesetfile
Wiki
```

#### 以行为单位的删除

删除第2-5行：

```bash
$ nl testfile | sed '2,5d'
     1  HELLO LINUX!  
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

删除第2行：

```bash
$ nl testfile | sed '2d' 
     1  HELLO LINUX!  
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

删除第3行到最后一行：

```bash
$ nl testfile | sed '3,$d' 
     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.
```

#### 添加内容

在第二行后添加文本：

```bash
$ nl testfile | sed '2a drink tea'
     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.  
drink tea
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

在第二行前添加文本：

```bash
$ nl testfile | sed '2i drink tea' 
     1  HELLO LINUX!  
drink tea
     2  Linux is a free unix-type opterating system.  
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

添加多行文本：

```bash
$ nl testfile | sed '2a Drink tea or ......\
drink beer ?'
     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.  
Drink tea or ......
drink beer ?
     3  This is a linux testfile!  
     4  Linux test 
     5  Google
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

#### 替换与显示

将第2-5行替换为指定文本：

```bash
$ nl testfile | sed '2,5c No 2-5 number'
     1  HELLO LINUX!  
No 2-5 number
     6  Taobao
     7  Runoob
     8  Tesetfile
     9  Wiki
```

只显示第5-7行：

```bash
$ nl testfile | sed -n '5,7p'
     5  Google
     6  Taobao
     7  Runoob
```

#### 数据的搜索与处理

搜索包含 `oo`的行并显示：

```bash
$ nl testfile | sed -n '/oo/p'
     5  Google
     7  Runoob
```

删除包含 `oo`的行：

```bash
$ nl testfile | sed '/oo/d'
     1  HELLO LINUX!  
     2  Linux is a free unix-type opterating system.  
     3  This is a linux testfile!  
     4  Linux test 
     6  Taobao
     8  Tesetfile
     9  Wiki
```

搜索 `oo`并执行替换和显示：

```bash
$ nl testfile | sed -n '/oo/{s/oo/kk/;p;q}'  
     5  Gkkgle
```

#### 查找与替换

替换每行第一次出现的 `oo`：

```bash
$ sed -e 's/oo/kk/' testfile
```

全局替换所有 `oo`：

```bash
$ sed -e 's/oo/kk/g' testfile
```

直接修改原文件：

```bash
$ sed -i 's/oo/kk/g' testfile
```

批量处理文件：

```bash
$ sed -i 's/oo/kk/g' ./test*
```

#### 实用示例

提取IP地址：

```bash
$ /sbin/ifconfig eth0 | grep 'inet addr' | sed 's/^.*addr://g' | sed 's/Bcast.*$//g'
192.168.1.100
```

#### 多点编辑

同时执行多条 `sed`命令：

```bash
$ nl testfile | sed -e '3,$d' -e 's/HELLO/RUNOOB/'
     1  RUNOOB LINUX!  
     2  Linux is a free unix-type opterating system.
```

#### 直接修改文件内容

在文件结尾 `.`替换为 `!`：

```bash
$ sed -i 's/\.$/\!/g' regular_express.txt
```

在最后一行添加内容：

```bash
$ sed -i '$a # This is a test' regular_express.txt
```

