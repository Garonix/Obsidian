# CMake 构建流程

CMake 的构建流程分为几个主要步骤，从设置项目到生成和执行构建命令。

1. **创建构建目录**：保持源代码目录整洁。
2. **使用 CMake 生成构建文件**：配置项目并生成适合平台的构建文件。
3. **编译和构建**：使用生成的构建文件执行编译和构建。
4. **清理构建文件**：删除中间文件和目标文件。
5. **重新配置和构建**：处理项目设置的更改。

![CMake构建流程2](../附件/CMake构建流程2.png)
以下是详细的构建流程说明：

## 1、创建构建目录

CMake 推荐使用 `"Out-of-source"` 构建方式，即将构建文件放在源代码目录之外的独立目录中。

这样可以保持源代码目录的整洁，并方便管理不同的构建配置。
![CMake目录组织](../附件/CMake目录组织.png)

**创建构建目录：**在项目的根目录下，创建一个新的构建目录。例如，可以创建一个名为 build 的目录。

```bash
mkdir build
```

**进入构建目录：**进入刚刚创建的构建目录。

```bash
cd build
```

## 2、使用 CMake 生成构建文件

在构建目录中运行 CMake，以生成适合当前平台的构建系统文件（例如 Makefile、Ninja 构建文件、Visual Studio 工程文件等）。

**运行 CMake 配置：**在构建目录中运行 CMake 命令，指定源代码目录。源代码目录是包含 CMakeLists.txt 文件的目录。

```bash
cmake ..
```

如果需要指定生成器（如 Ninja、Visual Studio），可以使用 -G 选项。例如：

```bash
cmake -G "Ninja" ..
```

如果需要指定构建类型（如 Debug 或 Release），可以使用 -DCMAKE_BUILD_TYPE 选项。例如：

```bash
cmake -DCMAKE_BUILD_TYPE=Release ..
```

**检查配置结果：**CMake 会输出配置过程中的详细信息，包括找到的库、定义的选项等，如果没有错误，构建系统文件将被生成到构建目录中。

## 3、编译和构建

使用生成的构建文件进行编译和构建。

不同的构建系统使用不同的命令。

**使用 Makefile（或类似构建系统）：**如果使用 Makefile，可以运行 make 命令来编译和构建项目。

```bash
make
```

如果要构建特定的目标，可以指定目标名称。例如：

```bash
make MyExecutable
```

**使用 Ninja：**如果使用 Ninja 构建系统，运行 ninja 命令来编译和构建项目。

```bash
ninja
```

与 make 类似，可以构建特定的目标：

```bash
ninja MyExecutable
```

**使用 Visual Studio：**如果生成了 Visual Studio 工程文件，可以打开 `.sln` 文件，然后在 Visual Studio 中选择构建解决方案。

也可以使用 msbuild 命令行工具来编译：

```bash
msbuild MyProject.sln /p:Configuration=Release
```

## 4、清理构建文件

构建过程中生成的中间文件和目标文件可以通过清理操作删除。

**使用 Makefile：**运行 make clean 命令（如果定义了清理规则）来删除生成的文件。

```bash
make clean
```

**使用 Ninja：**运行 ninja clean 命令（如果定义了清理规则）来删除生成的文件。

```bash
ninja clean
```

**手动删除：**可以手动删除构建目录中的所有文件，但保留源代码目录不变。例如：

```bash
rm -rf build/*
```

## 5、重新配置和构建

如果修改了 CMakeLists.txt 文件或项目设置，可能需要重新配置和构建项目。

**重新运行 CMake 配置：**在构建目录中重新运行 CMake 配置命令。

```bash
cmake ..
```

**重新编译：**使用构建命令重新编译项目。

```bash
make
```
