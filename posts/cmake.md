---
title: CMake
date: 2026-10-13 20:08:48
tags:
---

教程：[菜鸟教程](https://www.runoob.com/cmake/cmake-tutorial.html)，[CMake教程-翻译](https://cmake.com.cn/cmake/help/latest/guide/tutorial/index.html)，[Modern CMake](https://modern-cmake-cn.github.io/Modern-CMake-zh_CN/).

CMake 是一个跨平台的**构建系统生成工具**，用于管理 C/C++ 项目的编译过程，并不依赖于某特定编译器。

构建配置：

- CMakeLists.txt 文件：CMake 的配置文件，用于定义项目的构建规则、依赖关系、编译选项等。每个 CMake 项目通常包含一个或多个 CMakeLists.txt 文件。
- 构建目录：为了保持源代码的整洁，CMake 鼓励使用独立的构建目录（Out-of-source 构建，构建生成的文件与源代码分开存放。

工作流程：1.编写 CMakeLists.txt 文件，2.生成构建文件，3.执行构建。

### CMakeLists.txt

CMakeLists.txt 由一系列 CMake 指令组成，每个指令的格式为：**命令名(参数列表)**。CMake 通过读取文件中的指令来了解项目的结构、源文件列表和编译要求。下面是构建 CMake 项目时最常用的指令及其实例。

指定CMake最低版本：`cmake_minimum_required(VERSION <version>)`

```cmake
# 该指令必须放在 CMakeLists.txt 的最顶部
cmake_minimum_required(VERSION 3.10)
```

定义项目名称和语言：`project(<项目名> [<语言>...])`

```cmake
project(<项目名> [<语言>...])
```

添加可执行文件：`add_executable(<目标名> <源文件>...)`

```cmake
# 将指定的源文件编译生成一个可执行文件
add_executable(MyApp main.cpp utils.cpp)
```

添加库文件：`add_library(<目标名> [STATIC | SHARED | MODULE] <源文件>...)`

```cmake
# STATIC：静态库（.a / .lib），编译时直接嵌入可执行文件
# SHARED：动态库（.so / .dll），运行时加载
# 不指定类型时，由 BUILD_SHARED_LIBS 变量决定
add_library(MyLib STATIC library.cpp)
```

链接库到目标：`target_link_libraries(<目标> <库>...)`

```cmake
# 将指定的库链接到目标（可执行文件或其他库）
target_link_libraries(MyApp PRIVATE MyLib)
```

设置变量：`set(<变量名> <值>...)`

```cmake
# 定义一个普通变量，后续通过 ${变量名} 引用
set(CMAKE_CXX_STANDARD 17)

# 同时设置多个值（列表）
set(SOURCES main.cpp utils.cpp helper.cpp)
add_executable(MyApp ${SOURCES})
```

设置安装规则：

```cmake
# 语法：install(TARGETS <目标>...
#         [RUNTIME DESTINATION <可执行文件安装路径>]
#         [LIBRARY DESTINATION <动态库安装路径>]
#         [ARCHIVE DESTINATION <静态库安装路径>]
#         [INCLUDES DESTINATION <头文件安装路径>])
# 定义执行 make install 时各类型文件的安装位置
install(TARGETS MyApp RUNTIME DESTINATION bin)
```

条件语句：

```cmake
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    # Debug 模式下启用调试信息
    message(STATUS "当前为 Debug 构建模式")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
else()
    # Release 模式下启用优化
    message(STATUS "当前为 Release 构建模式")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
endif()
```

### 变量和缓存

CMake 使用变量来存储和传递配置信息，变量可以在 CMakeLists.txt 中定义和使用。变量分为两类：**普通变量**和**缓存变量**，它们的作用域和生命周期不同。

普通变量：在 CMakeLists.txt 中定义，作用域为当前目录及其子目录。在每次 CMake 运行时重新计算，不会持久化。

```cmake
# 定义一个字符串变量
set(MY_APP_NAME "runoob-app")

# 定义一个列表变量（用分号或空格分隔）
set(SOURCE_FILES main.cpp utils.cpp helper.cpp)

# 使用变量：通过 ${变量名} 引用
message(STATUS "项目名称：${MY_APP_NAME}")

# 使用列表变量：循环遍历每个源文件
foreach(FILE ${SOURCE_FILES})
    message(STATUS "源文件：${FILE}")
endforeach()
```

缓存变量：存储在 CMakeCache.txt 文件中，在多次 CMake 运行之间保持值不变。缓存变量通常用于让用户在 CMake 配置阶段自定义构建设置，例如开关功能或指定安装路径。*如果你修改了 CMakeLists.txt 中缓存变量的默认值，需要删除 CMakeCache.txt 或重新运行 cmake 才能让新默认值生效。*

```cmake
# 语法：set(<变量名> <默认值> CACHE <类型> <描述>)
# 类型可选：STRING、BOOL、PATH、FILEPATH

# 定义一个字符串类型的缓存变量（用户可修改的安装路径）
set(MY_INSTALL_PATH "/usr/local" CACHE PATH "程序的安装路径")

# 使用缓存变量和其他变量一样
if(ENABLE_LOGGING)
    message(STATUS "日志功能已启用")
    add_definitions(-DENABLE_LOGGING)
endif()
```

用户可以通过命令行 **-D** 参数修改缓存变量：

```cmake
# 在 cmake 命令中通过 -D 修改缓存变量
$ cmake .. -DMY_INSTALL_PATH=/opt/runoob -DENABLE_LOGGING=OFF
```

### 头文件搜索路径

在 CMake 中，有两种方式可以指定编译器查找头文件的路径：`include_directories()`和 `target_include_directories()`。两者都可以为编译器添加头文件搜索路径，但作用范围和控制粒度不同。

| 特性           | `include_directories()`                      | `target_include_directories()`           |
| :------------- | :------------------------------------------- | :--------------------------------------- |
| **作用范围**   | 全局作用域，影响当前目录及子目录中的所有目标 | 仅作用于指定的目标                       |
| **推荐程度**   | 不推荐，除非维护旧版 CMake 项目              | 推荐优先使用，符合现代 CMake 最佳实践    |
| **目标关联性** | 不直接关联到特定目标，可能意外影响其他目标   | 显式绑定到指定目标，依赖关系一目了然     |
| **可维护性**   | 较差，容易导致全局路径污染，问题难以排查     | 较好，路径与目标绑定，逻辑清晰           |
| **可见性控制** | 无法精确控制传播范围                         | 通过 PUBLIC、PRIVATE、INTERFACE 精确控制 |

`target_include_directories()`的关键优势在于可以通过可见性关键字精确控制头文件路径的传播。PUBLIC / PRIVATE / INTERFACE 的含义：

| 关键字      | 当前目标可用 | 依赖此目标的其他目标可用 | 典型场景                                |
| :---------- | :----------- | :----------------------- | :-------------------------------------- |
| `PRIVATE`   | 是           | 否                       | 仅自身实现需要的内部头文件              |
| `PUBLIC`    | 是           | 是                       | 库的公开 API 头文件，自身和消费者都需要 |
| `INTERFACE` | 否           | 是                       | 仅头文件的库（Header-only library）     |

### 查找库和包

CMake 提供了 `find_package()` 指令来自动检测和配置外部依赖。`find_package()` 会搜索系统中已安装的库，并设置对应的变量（如头文件路径、库文件路径、库目标名称）。

```cmake
# 基本用法：查找 Boost 库（不限定版本）
# REQUIRED 表示如果找不到则中止 CMake 配置
find_package(Boost REQUIRED)

# 指定最低版本：要求 Boost 版本不低于 1.70
find_package(Boost 1.70 REQUIRED)

# 指定搜索路径：在自定义路径下查找 OpenCV
find_package(OpenCV REQUIRED PATHS /path/to/opencv)
```

### CMake构建流程

1.创建构建目录。

2.生成构建文件。

CMake 支持多种构建系统生成器，如 Unix Makefiles、Ninja、Visual Studio、Xcode 等。

基本用法——生成默认构建文件：

```shell
# .. 指向包含 CMakeLists.txt 的源代码目录
cmake ..
```

指定生成器：

使用 `-G` 参数可以指定要生成的构建系统类型。

```shell
# 使用 Ninja 作为构建系统（速度比 Make 更快）
cmake -G "Ninja" ..
```

指定构建类型：使用 `-DCMAKE_BUILD_TYPE` 参数设置编译优化级别和调试信息。

```shell
# 四种常见的构建类型：Debug / Release / RelWithDebInfo / MinSizeRel
cmake -DCMAKE_BUILD_TYPE=Release ..
```

检查配置结果：CMake 会输出配置过程中的详细信息，包括检测到的编译器、找到的库、定义的选项等。

3.编译和构建。构建文件生成后，就可以执行实际的编译和链接过程了。不同的构建系统使用不同的编译命令，你也可以使用 CMake 提供的通用构建命令。

推荐方式——使用 CMake 通用构建命令：

```shell
# 无论底层使用哪种构建系统，该命令都能正确编译
cmake --build .
```

构建特定目标：

```shell
# 只编译指定的目标（如某个可执行文件或库）
cmake --build . --target MyExecutable
```

使用 Makefile 构建系统：

```shell
# 编译所有目标
make

# 使用多核并行编译（-j 参数指定并行任务数）
make -j$(nproc)

# 编译特定目标
make MyExecutable
```

使用 Ninja 构建系统：

```shell
# Ninja 自动并行编译，无需手动指定 -j
ninja

# 编译特定目标
ninja MyExecutable
```

使用 Visual Studio：

如果生成了 Visual Studio 工程文件，可以打开 `.sln` 解决方案文件，在 IDE 中选择生成解决方案。

```shell
# 也可以使用 MSBuild 命令行工具编译
msbuild MyProject.sln /p:Configuration=Release
```

CMake 支持生成多种构建系统文件，以下是三种最常用构建系统的对比。

| 构建系统       | 适用平台      | 并行编译        | 特点                                     |
| :------------- | :------------ | :-------------- | :--------------------------------------- |
| Unix Makefiles | Linux / macOS | 需手动指定 `-j` | 兼容性最广，几乎所有 Unix 系统自带       |
| Ninja          | 跨平台        | 自动并行        | 速度更快，特别适合增量编译和大型项目     |
| Visual Studio  | Windows       | IDE 自动管理    | 与 VS IDE 深度集成，支持图形化调试和配置 |
