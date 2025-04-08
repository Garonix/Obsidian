1.str存在内部计数器str.count('匹配模板')
2.利用set去重
3.利用enumerate遍历迭代器
for index, value in enumerate(my_list):
4.可以使用range(首，尾，步长)方法进行一些规定了步长的分割操作
5.format方法
"字符串内容{占位符}".format(要插入的值1, 要插入的值2, ...)
占位符中的语法
{参数索引或参数名:填充字符 对齐方式 符号 宽度 精度 类型}
6.字典相关

- `len(my_dict)`: 返回字典中键值对的数量。
- `my_dict.keys()`: 返回字典中所有键的列表。
- `my_dict.values()`: 返回字典中所有值的列表。
- `my_dict.items()`: 返回字典中所有键值对的列表，每个键值对都是一个元组。
- del my_dict["key1"]：删除键值对
7.sort()方法（只支持列表）默认升序，可以使用sorted()方法（支持其他可迭代对象）保留原来的列表
8.lambda函数
lambda 参数列表: 表达式
9.int(\*\*\*，进制)直接进制转换，转换后的**字符串**会带有进制前缀：`0b` (二进制)、`0o` (八进制)、`0x` (十六进制)。
10.filter()方法
filter(function, iterable)，返回由满足条件的元素组成的新迭代器
- `function`：是一个函数，用于判断序列中的每个元素是否满足条件。
- `iterable`：是一个可迭代对象，例如列表、元组等。
11.