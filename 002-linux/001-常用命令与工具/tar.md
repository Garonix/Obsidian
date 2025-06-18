- **`tar -cvf archive.tar directory/`**
    
    - 作用：创建新的 tar 归档文件，包含指定目录的内容。`-c` 创建，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -cvf my_archive.tar my_directory/`
- **`tar -xvf archive.tar`**
    
    - 作用：解压 tar 归档文件。`-x` 解压，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -xvf my_archive.tar`
- **`tar -tvf archive.tar`**
    
    - 作用：列出 tar 归档文件中的内容，但不解压。`-t` 列出，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -tvf my_archive.tar`
- **`tar -czvf archive.tar.gz directory/`**
    
    - 作用：创建使用 gzip 压缩的 tar 归档文件 (tar.gz)。`-c` 创建，`-z` 使用 gzip 压缩，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -czvf my_archive.tar.gz my_directory/`
- **`tar -xzvf archive.tar.gz`**
    
    - 作用：解压使用 gzip 压缩的 tar 归档文件 (tar.gz)。`-x` 解压，`-z` 使用 gzip 解压缩，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -xzvf my_archive.tar.gz`
- **`tar -cjvf archive.tar.bz2 directory/`**
    
    - 作用：创建使用 bzip2 压缩的 tar 归档文件 (tar.bz2)。`-c` 创建，`-j` 使用 bzip2 压缩，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -cjvf my_archive.tar.bz2 my_directory/`
- **`tar -xjvf archive.tar.bz2`**
    
    - 作用：解压使用 bzip2 压缩的 tar 归档文件 (tar.bz2)。`-x` 解压，`-j` 使用 bzip2 解压缩，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -xjvf my_archive.tar.bz2`
- **`tar -cJvf archive.tar.xz directory/`**
    
    - 作用：创建使用 xz 压缩的 tar 归档文件 (tar.xz)。`-c` 创建，`-J` 使用 xz 压缩，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -cJvf my_archive.tar.xz my_directory/`
- **`tar -xJvf archive.tar.xz`**
    
    - 作用：解压使用 xz 压缩的 tar 归档文件 (tar.xz)。`-x` 解压，`-J` 使用 xz 解压缩，`-v` 显示详情，`-f` 指定归档文件名。
    - 示例：`tar -xJvf my_archive.tar.xz`