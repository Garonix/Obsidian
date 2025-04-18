`awk` 是一种处理文本文件的语言，是一个强大的文本分析工具。

`awk` 通过提供编程语言的功能，如变量、数学运算、字符串处理等，使得对文本文件的分析和操作变得非常灵活和高效。

之所以叫 `awk` 是因为其取了三位创始人 Alfred Aho，Peter Weinberger, 和 Brian Kernighan 的 Family Name 的首字符。

## 语法

```
awk options 'pattern {action}' file
```

**选项参数说明：**

- `options`：控制 `awk` 的行为选项
- `pattern`：用于匹配输入数据的模式，省略则对所有行操作
- `{action}`：在匹配到模式的行上执行的动作，省略则默认打印整行

**options 参数：**

- `-F <分隔符>` 或 `--field-separator=<分隔符>`：指定字段分隔符，默认是空格
- `-v <变量名>=<值>`：设置 `awk` 内部变量值，将外部值传递给脚本
- `-f <脚本文件>`：指定包含 `awk` 脚本的文件
- `-V` 或 `--version`：显示版本信息
- `-h` 或 `--help`：显示帮助信息

## 常见用法示例

### 打印整行

```bash
awk '{print}' file
```

### 打印特定列

```bash
awk '{print $1, $2}' file
```

### 使用分隔符指定列

```bash
awk -F',' '{print $1, $2}' file
```

### 打印行数

```bash
awk '{print NR, $0}' file
```

### 打印满足条件的行

```bash
awk '/pattern/ {print NR, $0}' file
```

### 计算列的总和

```bash
awk '{sum += $1} END {print sum}' file
```

### 打印最大值

```bash
awk 'max < $1 {max = $1} END {print max}' file
```

### 格式化输出

```bash
awk '{printf "%-10s %-10s\n", $1, $2}' file
```

## 基本用法

log.txt 文本内容如下：

```
2 this is a test
3 Do you like awk
This's a test
10 There are orange,apple,mongo
```

### 用法一：基本模式匹配和动作

```bash
# 每行按空格或TAB分割，输出文本中的1、4项
awk '{print $1,$4}' log.txt
```

输出：

```
2 a
3 like
This's
10 orange,apple,mongo
```

```bash
# 格式化输出
awk '{printf "%-8s %s\n", $1, $4}' log.txt
```

输出：

```
2        a
3        like
This's   
10       orange,apple,mongo
```

### 用法二：指定分隔符

```bash
# 使用","分割
awk -F, '{print $1,$2}' log.txt
```

输出：

```
2 this is a test
3 Do you like awk
This's a test
10 There are orange apple
```

```bash
# 使用内建变量
awk 'BEGIN{FS=","} {print $1,$2}' log.txt
```

```bash
# 使用多个分隔符：先空格分割，再","分割
awk -F '[ ,]' '{print $1,$3}' log.txt
```

输出：

```
2 this test
3 Are awk
This's a
10 There apple
```

### 用法三：设置变量

```bash
awk -va=1 '{print $1, $1+a}' log.txt
```

输出：

```
2 3
3 4
This's 1
10 11
```

```bash
awk -va=1 -vb=s '{print $1, $1+a, $1b}' log.txt
```

### 用法四：使用 awk 脚本文件

```bash
awk -f cal.awk log.txt
```

## 运算符

| 运算符                  | 描述                             |
| ----------------------- | -------------------------------- |
| = += -= *= /= %= ^= **= | 赋值                             |
| ?:                      | C条件表达式                      |
| \|\|                    | 逻辑或                           |
| &&                      | 逻辑与                           |
| ~ 和 !~                 | 匹配正则表达式和不匹配正则表达式 |
| < <= > >= != ==         | 关系运算符                       |
| 空格                    | 连接                             |
| + -                     | 加，减                           |
| * / %                   | 乘，除与求余                     |
| + - !                   | 一元加，减和逻辑非               |
| ^ ***                   | 求幂                             |
| ++ --                   | 增加或减少，作为前缀或后缀       |
| $                       | 字段引用                         |
| in                      | 数组成员                         |

### 过滤示例

```bash
# 过滤第一列大于2的行
awk '$1>2' log.txt
```

```bash
# 过滤第一列等于2的行
awk '$1==2 {print $1,$3}' log.txt
```

```bash
# 过滤第一列大于2且第二列等于'Are'的行
awk '$1>2 && $2=="Are" {print $1,$2,$3}' log.txt
```

## 内建变量

| 变量        | 描述                                              |
| ----------- | ------------------------------------------------- |
| $n          | 当前记录的第n个字段，字段间由FS分隔               |
| $0          | 完整的输入记录                                    |
| ARGC        | 命令行参数的数目                                  |
| ARGIND      | 命令行中当前文件的位置(从0开始算)                 |
| ARGV        | 包含命令行参数的数组                              |
| CONVFMT     | 数字转换格式(默认值为%.6g)ENVIRON环境变量关联数组 |
| ERRNO       | 最后一个系统错误的描述                            |
| FIELDWIDTHS | 字段宽度列表(用空格键分隔)                        |
| FILENAME    | 当前文件名                                        |
| FNR         | 各文件分别计数的行号                              |
| FS          | 字段分隔符(默认是任何空格)                        |
| IGNORECASE  | 如果为真，则进行忽略大小写的匹配                  |
| NF          | 一条记录的字段的数目                              |
| NR          | 已经读出的记录数，就是行号，从1开始               |
| OFMT        | 数字的输出格式(默认值是%.6g)                      |
| OFS         | 输出字段分隔符，默认值与输入字段分隔符一致        |
| ORS         | 输出记录分隔符(默认值是一个换行符)                |
| RLENGTH     | 由match函数所匹配的字符串的长度                   |
| RS          | 记录分隔符(默认是一个换行符)                      |
| RSTART      | 由match函数所匹配的字符串的第一个位置             |
| SUBSEP      | 数组下标分隔符(默认值是/034)                      |

```bash
awk 'BEGIN{printf "FILENAME ARGC FNR FS NF NR OFS ORS RS\n"} {printf "%s %d %d %s %d %d %s %s %s\n", FILENAME, ARGC, FNR, FS, NF, NR, OFS, ORS, RS}' log.txt
```

## 使用正则，字符串匹配

```bash
# 输出第二列包含 "th"，并打印第二列与第四列
awk '$2 ~ /th/ {print $2,$4}' log.txt
```

输出：

```
this a
```

**~ 表示模式开始。// 中是模式。**

```bash
# 输出包含 "re" 的行
awk '/re/' log.txt
```

## 忽略大小写

```bash
awk 'BEGIN{IGNORECASE=1} /this/' log.txt
```

## 模式取反

```bash
awk '$2 !~ /th/' log.txt
```

```bash
awk '!/th/' log.txt
```

## awk 脚本

关于 `awk` 脚本，我们需要注意两个关键词 `BEGIN` 和 `END`：

- `BEGIN{ 这里面放的是执行前的语句 }`
- `END {这里面放的是处理完所有的行后要执行的语句 }`
- `{这里面放的是处理每一行时要执行的语句}`

假设有这么一个文件（学生成绩表）：

```
Marry   2143 78 84 77
Jack    2321 66 78 45
Tom     2122 48 77 71
Mike    2537 87 97 95
Bob     2415 40 57 62
```

我们的 `awk` 脚本如下：

```awk
#!/bin/awk -f
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0

    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}
```

## 其它实例

### AWK 的 hello world 程序

```awk
BEGIN { print "Hello, world!" }
```

### 计算文件大小

```bash
ls -l *.txt | awk '{sum+=$5} END {print sum}'
```

### 从文件中找出长度大于 80 的行

```bash
awk 'length>80' log.txt
```

### 打印九九乘法表

```bash
seq 9 | sed 'H;g' | awk -v RS='' '{for(i=1;i<=NF;i++)printf("%dx%d=%d%s", i, NR, i*NR, i==NR?"\n":"\t")}'
```

## 参考资源

> 更多内容：
>
> - [AWK 工作原理](https://www.runoob.com/w3cnote/awk-work-principle.html)
> - [AWK 数组](https://www.runoob.com/w3cnote/awk-arrays.html)
> - [AWK 条件语句与循环](https://www.runoob.com/w3cnote/awk-if-loop.html)
> - [AWK 用户自定义函数](https://www.runoob.com/w3cnote/awk-user-defined-functions.html)
> - [AWK 内置函数](https://www.runoob.com/w3cnote/awk-built-in-functions.html)
> - [8 个有力的 Awk 内建变量](https://www.runoob.com/w3cnote/8-awesome-awk-built-in-variables.html)
> - [AWK 官方手册](http://www.gnu.org/software/gawk/manual/gawk.html)

[![Linux 命令大全](https://www.runoob.com/images/up.gif) Linux 命令大全](https://www.runoob.com/linux/linux-command-manual.html)
