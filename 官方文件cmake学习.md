## 构建可执行文件
0. 
`cmake_minimum_required()`
`project()`

1. 创建一个目标
命令`add_executable()`  
在 CMake 术语中，目标是开发者为一组属性指定的名称。  
目标可能需要跟踪的一些属性示例包括：  
- 工件类型（可执行文件、库、头文件集合等）  
- 源文件
- 包含目录
- 可执行文件或库的输出名称
- 依赖关系
目标本身只是名称，是指向这组属性的句柄。使用`add_executable()`命令非常简单，只需指定我们要用于目标的名称即可。
`add_executable(MyProgram)`
2. 构建和链接的源文件。
主要命令是：`target_sources()`，它接受目标名称作为参数，后跟一个或多个文件集合
```bash
target_sources
(MyProgram
  PRIVATE
  main.cxx
)
```
*CMake 变量 (CMAKE_CURRENT_SOURCE_DIR) 存储了当前正在处理的 CMakeLists.txt 文件所在的目录的绝对路径。*
作用域关键字 (<scope>): PRIVATE、INTERFACE 和 PUBLIC  
它们定义了依赖关系中属性（如源文件、头文件路径、定义宏）的可见性和传递性。  
除了高级和特殊用法之外，可执行文件的 scope 关键字应该始终为PRIVATE`<scope>`。对于实现文件，无论目标是可执行文件还是库，这条规则也同样适用。
**唯一需要“看到”这些 .cxx文件的目标就是构建它们的目标。**

3. 配置和构建
1) 配置阶段：cmake -B build  
这是 配置 阶段，CMake 会分析项目、环境并生成构建系统文件。
|命令部分|	作用|	解释|
|---|---|---|
|cmake|	调用 CMake 程序。|	
|-B build|	指定构建目录 (Build Directory)。|	这个选项告诉 CMake 在当前目录下创建一个名  为 build 的子目录（如果不存在），并将所有生成的构建文件（如 Makefile、CMakeCache.txt 等）放入其中。这是一种推荐的 out-of-source 构建方式。|
|隐含行为|	指定源码目录 (Source Directory)。	|当您没有指定源码目录时，CMake 默认使用当前执行命令的目录 作为源码目录。|
|隐含行为|	指定生成器 (Generator)。|	当您没有使用 -G 选项时，CMake 会根据操作系统自动选择默认生成器，通常是 Unix Makefiles (Linux/macOS) 或 Visual Studio (Windows)。  |  

该-B标志指示 CMake 使用指定的相对路径作为构建过程中生成文件和存储构建产物的位置。如果省略该标志，则使用当前工作目录。通常认为将生成的文件放在源代码树中是一种不好的做法，即进行“源代码内”构建(生成的文件会“污染”源文件)。  


2) 构建阶段：
`cmake --build build`这是 构建 阶段，CMake 会调用实际的底层构建工具（如 make 或 ninja）来执行编译和链接操作。命令部分作用解释cmake再次调用 CMake 程序。  
`--build`触发构建操作。这是 CMake 提供的、跨平台的构建接口。它会读取构建目录中的配置信息。  
`build`指定要构建的目录。CMake 会进入 build 目录，确定之前使用的是哪个生成器（比如 make），然后自动运行相应的构建命令（比如在后台执行 make）。  
总结： 此命令会使用上一步配置中确定的构建系统，编译您的源代码并生成最终的可执行文件或库。为什么使用 cmake --build？使用 cmake --build 而不是直接调用 make 或 ninja 的主要优势是 跨平台一致性。无论您在 Linux (使用 make) 还是 Windows (使用 MSBuild)，您都可以使用同一条命令进行构建，而无需记住底层构建工具的具体名称或参数。

