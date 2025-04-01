# CMakeLists.txt 文件说明

CMakeLists.txt 是 CMake 的配置文件，用于定义项目的构建规则、依赖关系、编译选项等。每个 CMake 项目通常都有一个或多个 CMakeLists.txt 文件。

## 文件结构和基本语法

CMakeLists.txt 文件使用一系列的 CMake 指令来描述构建过程。常见的指令包括：

**指定 CMake 的最低版本要求：**

  ```cmake
  cmake_minimum_required(VERSION <version>)
  ```

  例如：

  ```cmake
  cmake_minimum_required(VERSION 3.10)
  ```

**定义项目的名称和使用的编程语言：**

  ```cmake
  project(<project_name> [<language>...])
  ```

  例如：

  ```cmake
  project(MyProject CXX)
  ```

**指定要生成的可执行文件和其源文件：**

  ```cmake
  add_executable(<target> <source_files>...)
  ```

  例如：

  ```cmake
  add_executable(MyExecutable main.cpp other_file.cpp)
  ```

**创建一个库（静态库或动态库）及其源文件：**

  ```cmake
  add_library(<target> <source_files>...)
  ```

  例如：

  ```cmake
  add_library(MyLibrary STATIC library.cpp)
  ```

**链接目标文件与其他库：**

  ```cmake
  target_link_libraries(<target> <libraries>...)
  ```

  例如：

  ```cmake
  target_link_libraries(MyExecutable MyLibrary)
  ```

**添加头文件搜索路径：**

  ```cmake
  include_directories(<dirs>...)
  ```

  例如：

  ```cmake
  include_directories(${PROJECT_SOURCE_DIR}/include)
  ```

**设置变量的值：**

  ```cmake
  set(<variable> <value>...)
  ```

  例如：

  ```cmake
  set(CMAKE_CXX_STANDARD 11)
  ```

**设置目标属性：**

  ```cmake
  target_include_directories(TARGET target_name
                [BEFORE | AFTER]
                [SYSTEM] [PUBLIC | PRIVATE | INTERFACE]
                [items1...])
  ```

  例如：

  ```cmake
  target_include_directories(MyExecutable PRIVATE ${PROJECT_SOURCE_DIR}/include)
  ```

**安装规则：**

  ```cmake
  install(TARGETS target1 [target2 ...]
      [RUNTIME DESTINATION dir]
      [LIBRARY DESTINATION dir]
      [ARCHIVE DESTINATION dir]
      [INCLUDES DESTINATION [dir ...]]
      [PRIVATE_HEADER DESTINATION dir]
      [PUBLIC_HEADER DESTINATION dir])
  ```

  例如：

  ```cmake
  install(TARGETS MyExecutable RUNTIME DESTINATION bin)
  ```

**条件语句 (if, elseif, else, endif 命令)：**

  ```cmake
  if(expression)
    # Commands
  elseif(expression)
    # Commands
  else()
    # Commands
  endif()
  ```

  例如：

  ```cmake
  if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message("Debug build")
  endif()
  ```

**自定义命令 (add_custom_command 命令)：**

  ```cmake
  add_custom_command(
     TARGET target
     PRE_BUILD | PRE_LINK | POST_BUILD
     COMMAND command1 [ARGS] [WORKING_DIRECTORY dir]
     [COMMAND command2 [ARGS]]
     [DEPENDS [depend1 [depend2 ...]]]
     [COMMENT comment]
     [VERBATIM]
  )
  ```

  例如：

  ```cmake
  add_custom_command(
     TARGET MyExecutable POST_BUILD
     COMMAND ${CMAKE_COMMAND} -E echo "Build completed."
  )
  ```

### 实例

一个简单的 CMakeLists.txt 文件示例：

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject CXX)

# 添加源文件
add_executable(MyExecutable main.cpp)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
```

### 变量和缓存

CMake 使用变量来存储和传递信息，这些变量可以在 CMakeLists.txt 文件中定义和使用。

变量可以分为普通变量和缓存变量。

#### 变量定义与使用

**定义变量：**

```cmake
set(MY_VAR "Hello World")
```

**使用变量：**

```cmake
message(STATUS "Variable MY_VAR is ${MY_VAR}")
```

#### 缓存变量

缓存变量存储在 CMake 的缓存文件中，用户可以在 CMake 配置时修改这些值。缓存变量通常用于用户输入的设置，例如编译选项和路径。

**定义缓存变量：**

```cmake
set(MY_CACHE_VAR "DefaultValue" CACHE STRING "A cache variable")
```

**使用缓存变量：**

```cmake
message(STATUS "Cache variable MY_CACHE_VAR is ${MY_CACHE_VAR}")
```

### 查找库和包

CMake 可以通过 `find_package()` 指令自动检测和配置外部库和包。

常用于查找系统安装的库或第三方库。

#### find_package() 指令

基本用法：

```cmake
find_package(Boost REQUIRED)
```

指定版本：

```cmake
find_package(Boost 1.70 REQUIRED)
```

查找库并指定路径：

```cmake
find_package(OpenCV REQUIRED PATHS /path/to/opencv)
```

使用查找到的库：

```cmake
target_link_libraries(MyExecutable Boost::Boost)
```

设置包含目录和链接目录：

```cmake
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})
```

#### 使用第三方库

假设你想在项目中使用 Boost 库，CMakeLists.txt 文件可能如下所示：

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject CXX)

# 查找 Boost 库
find_package(Boost REQUIRED)

# 添加源文件
add_executable(MyExecutable main.cpp)

# 链接 Boost 库
target_link_libraries(MyExecutable Boost::Boost)
```

通过上述内容，用户可以了解 CMakeLists.txt 文件的基本结构和常用指令，掌握如何定义和使用变量，查找和配置外部库，从而能够有效地使用 CMake 管理项目构建过程。
