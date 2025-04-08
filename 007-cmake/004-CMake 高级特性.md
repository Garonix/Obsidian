CMake 高级特性允许我们更灵活地管理和配置 CMake 项目，以适应复杂的构建需求和环境。

1.  **自定义 CMake 模块和脚本**：创建自定义模块和脚本以简化构建过程。
2.  **构建配置和目标**：使用多配置生成器和定义多个构建目标。
3.  **高级查找和配置**：灵活地查找包和配置构建选项。
4.  **生成自定义构建步骤**：添加自定义命令和目标以执行额外的构建操作。
5.  **跨平台和交叉编译**：支持不同平台的构建和交叉编译。
6.  **目标属性和配置**：设置和修改目标的属性，以满足特定需求。

1、自定义 CMake 模块和脚本
-----------------

### 1.1 自定义 CMake 模块

CMake 允许你创建和使用自定义模块，以简化常见的构建任务。

自定义模块通常包含自定义的 CMake 脚本和函数。

**创建自定义模块：**

*   在项目目录下创建一个 `cmake/` 目录，用于存放自定义 CMake 模块。
*   在 `cmake/` 目录下创建一个 `MyModule.cmake` 文件。
*   在 `CMakeLists.txt` 文件中包含自定义模块：
    
    ```cmake
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
    include(MyModule)
    ```
    

**自定义模块示例 (MyModule.cmake)：**

```cmake
function(my_custom_function)
  message(STATUS "This is a custom function!")
endfunction()
```

在 CMakeLists.txt 中调用自定义函数：

```cmake
my_custom_function()
```

### 1.2 使用自定义 CMake 脚本

自定义 CMake 脚本允许你执行自定义配置操作，灵活处理复杂的构建要求。

**创建自定义脚本：**

*   在项目中创建一个脚本文件（例如 `config.cmake`）。
*   在脚本中编写 CMake 指令。

在 CMakeLists.txt 文件中调用脚本：

```cmake
include(${CMAKE_SOURCE_DIR}/config.cmake)
```

2、构建配置和目标
---------

### 2.1 多配置生成器

CMake 支持多种构建配置（如 Debug、Release）。

多配置生成器允许你在同一构建目录中同时生成不同配置的构建。

**指定配置类型**

在 CMakeLists.txt 中设置默认配置：

```cmake
set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Build type")
```

使用 Visual Studio：

在 Visual Studio 中选择构建配置（Debug 或 Release）。

### 2.2 构建目标

你可以定义多个构建目标，每个目标可以有不同的构建设置和选项。

添加多个目标：

```cmake
add_executable(MyExecutable1 src/main1.cpp)
add_executable(MyExecutable2 src/main2.cpp)
```

设置目标属性：

```cmake
set_target_properties(MyExecutable1 PROPERTIES COMPILE_DEFINITIONS "DEBUG")
set_target_properties(MyExecutable2 PROPERTIES COMPILE_DEFINITIONS "RELEASE")
```

3、高级查找和配置
---------

### 3.1 查找包的高级用法

find_package() 指令可以用于查找和配置复杂的第三方库和包。

查找包的高级选项：

```cmake
find_package(Boost REQUIRED COMPONENTS filesystem system)
```

设置查找路径：

```cmake
set(BOOST_ROOT "/path/to/boost")
find_package(Boost REQUIRED)
```

### 3.2 配置文件和构建选项

你可以通过 CMake 配置文件来控制构建选项和配置。

配置选项：

```cmake
configure_file(config.h.in config.h)
```

配置文件 (config.h.in)：

```cmake
#define VERSION "@PROJECT_VERSION@"
```

在源文件中包含配置文件：

```cmake
#include "config.h"
```

4、生成自定义构建步骤
-----------

### 4.1 自定义命令

CMake 允许你添加自定义构建命令，以便在构建过程中执行额外的操作。

添加自定义命令：

```cmake
add_custom_command(
  OUTPUT ${CMAKE_BINARY_DIR}/generated_file.txt
  COMMAND ${CMAKE_COMMAND} -E echo "Generating file" > ${CMAKE_BINARY_DIR}/generated_file.txt
  DEPENDS ${CMAKE_SOURCE_DIR}/input_file.txt
)
```

添加自定义目标：

```cmake
add_custom_target(generate_file ALL
  DEPENDS ${CMAKE_BINARY_DIR}/generated_file.txt
)
```

### 4.2 自定义目标

自定义目标可以用来执行自定义构建步骤，如生成代码、处理资源等。

创建自定义目标：

```cmake
add_custom_target(my_target
  COMMAND ${CMAKE_COMMAND} -E echo "Running custom target"
  DEPENDS some_dependency
)
```

在构建过程中执行目标：

```bash
cmake --build . --target my_target
```

5、跨平台和交叉编译
----------

### 5.1 跨平台构建

CMake 支持多平台构建，允许你为不同操作系统生成适当的构建文件。

指定平台：

```bash
cmake -DCMAKE_SYSTEM_NAME=Linux ..
```

### 5.2 交叉编译

CMake 支持交叉编译，即为不同的架构或平台构建项目。

指定工具链文件：

```bash
cmake -DCMAKE_TOOLCHAIN_FILE=/path/to/toolchain.cmake ..
```

工具链文件示例 (toolchain.cmake)：

```cmake
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)
```

6、目标属性和配置
---------

### 6.1 目标属性

设置和修改目标的属性，如编译选项、链接选项等。

设置编译选项：

```cmake
set_target_properties(MyExecutable PROPERTIES COMPILE_OPTIONS "-Wall")
```

设置链接选项：

```cmake
set_target_properties(MyExecutable PROPERTIES LINK_FLAGS "-L/path/to/lib")
```

### 6.2 自定义编译和链接选项

为特定目标设置自定义的编译和链接选项。

设置编译选项：

```cmake
target_compile_options(MyExecutable PRIVATE -Wall -Wextra)
```

设置链接选项：

```cmake
target_link_options(MyExecutable PRIVATE -L/path/to/lib)
```