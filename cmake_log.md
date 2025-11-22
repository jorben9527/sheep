cmake -S <dir>
指定项目根目录，CMake 将在此目录中查找要构建的项目。此目录包含根CMakeLists.txt文件，我们将在教程的第一步中讨论该文件。

如果未指定，则默认为当前工作目录。

cmake -B <dir>
指定构建目录，CMake 会将生成的构建系统的文件以及构建系统运行时生成的构建产物输出到该目录。

如果未指定，则默认为当前工作目录。

cmake --build <dir>
在指定的构建目录中运行构建系统。这是一个适用于所有生成器的通用命令。对于多配置生成器，可以通过以下方式请求所需的配置：

cmake --build <dir> --config <cfg>

```bash
cmake_minimum_required (VERSION 2.8)

project (demo)

add_executable(main main.c)
```
`aux_source_directory(. SRC_LIST`把当前目录下的源文件存列表存放到变量SRC_LIST里;

aux_source_directory()也存在弊端，它会把指定目录下的所有源文件都加进来，可能会加入一些我们不需要的文件，此时我们可以使用set命令去新建变量来存放需要的源文件，如下：
```bash

set( SRC_LIST
	 ./main.c
	 ./testFunc1.c
	 ./testFunc.c)

```
`include_directories`该命令是用来向工程添加多个指定头文件的搜索路径，路径之间用空格分隔。

`add_subdirectory`：这个语句的作用是增加编译子目录。

EXECUTABLE_OUTPUT_PATH ：目标二进制可执行文件的存放位置
PROJECT_SOURCE_DIR：工程的根目录

add_library: 生成动态库或静态库(第1个参数指定库的名字；第2个参数决定是动态还是静态，如果没有就默认静态；第3个参数指定生成库的源文件)
```bash
add_library (testFunc_shared SHARED ${SRC_LIST})
add_library (testFunc_static STATIC ${SRC_LIST})

set_target_properties (testFunc_shared PROPERTIES OUTPUT_NAME "testFunc")
set_target_properties (testFunc_static PROPERTIES OUTPUT_NAME "testFunc")

```
set_target_properties: 设置最终生成的库的名称  
前面使用set_target_properties重新定义了库的输出名称，如果不使用set_target_properties也可以，那么库的名称就是add_library里定义的名称，只是连续2次使用add_library指定库名称时（第一个参数），这个名称不能相同，而set_target_properties可以把名称设置为相同，只是最终生成的库文件后缀不同（一个是.so，一个是.a），这样相对来说会好看点。

find_library: 在指定目录下查找指定库，并把库的绝对路径存放到变量里，其第一个参数是变量名称，第二个参数是库名称，第三个参数是HINTS，第4个参数是路径，其它用法可以参考cmake文档

target_link_libraries: 把目标文件与库文件进行链接