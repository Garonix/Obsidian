# CMake 简介

## 1. CMake是什么

CMake 是个一个开源的**跨平台**自动化构建系统，用来管理软件建置的程序，**并不依赖于某特定编译器**，并可支持多层目录、多个应用程序与多个函数库。

>CMake 本身不是构建工具，而是生成构建系统的工具，它生成的构建系统可以使用不同的编译器和工具链。  

## 2. 基本工作流程

### 编写 CMakeLists.txt 文件

定义项目的构建规则和依赖关系

### 生成构建文件

使用 CMake 生成适合当前平台的构建系统文件
>如 Makefile、Visual Studio 工程文件

### 执行构建

使用生成的构建系统文件来编译项目
>如 make、ninja、msbuild

<img src = "https://camo.githubusercontent.com/76f83b91cac93e85ed2e6f7c0bf075c52d9c84333ad7660cfe86632be4fac761/68747470733a2f2f696d672e7a656765732e746f702f483741784c5a2e706e67" width = "500px">
![CMake构建流程1](../附件/CMake构建流程1.png|100x200)
