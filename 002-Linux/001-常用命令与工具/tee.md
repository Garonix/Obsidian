- **`tee <文件名>`**
    
    - 作用：将标准输入的数据同时输出到指定文件和标准输出。
    - 示例：`echo "Hello, World!" | tee output.txt`，该命令会将 "Hello, World!" 输出到终端屏幕，同时写入到`output.txt`文件中。
- **`tee -a <文件名>`**
    
    - 作用：将标准输入的数据以追加的方式同时输出到指定文件和标准输出。
    - 示例：`echo "This is a new line" | tee -a output.txt`，会把 "This is a new line" 输出到终端，并追加写入到`output.txt`文件中。
- **`command | tee <文件名>`**
    
    - 作用：将命令的输出结果同时输出到指定文件和标准输出。
    - 示例：`ls -l | tee list.txt`，会把`ls -l`命令的执行结果输出到终端屏幕，同时保存到`list.txt`文件中。
- **`command | tee -a <文件名>`**
    
    - 作用：将命令的输出结果以追加的方式同时输出到指定文件和标准输出。
    - 示例：`date | tee -a log.txt`，会把当前日期和时间的输出结果显示在终端，并追加到`log.txt`文件中。
- **`tee <文件1> <文件2> ...`**
    
    - 作用：将标准输入的数据同时输出到多个指定文件和标准输出。
    - 示例：`echo "Some text" | tee file1.txt file2.txt`，会把 "Some text" 输出到终端屏幕，同时分别写入到`file1.txt`和`file2.txt`文件中。
- **`tee -a <文件1> <文件2> ...`**
    
    - 作用：将标准输入的数据以追加的方式同时输出到多个指定文件和标准输出。
    - 示例：`echo "More text" | tee -a file1.txt file2.txt`，会把 "More text" 输出到终端屏幕，并分别追加到`file1.txt`和`file2.txt`文件中。