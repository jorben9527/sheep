## 构建可执行文件
0. 
```bash
cmake_minimum_required(VERSION 3.23)

project(MyProjectName)
```
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
1)  配置阶段：`cmake -B build ` 
这是 配置 阶段，CMake 会分析项目、环境并生成构建系统文件。
|命令部分|	作用|	解释|
|---|---|---|
|cmake|	调用 CMake 程序。|	
|-B build|	指定构建目录 (Build Directory)。|	这个选项告诉 CMake 在当前目录下创建一个名  为 build 的子目录（如果不存在），并将所有生成的构建文件（如 Makefile、CMakeCache.txt 等）放入其中。这是一种推荐的 out-of-source 构建方式。|
|隐含行为|	指定源码目录 (Source Directory)。|	当您没有指定源码目录时，CMake 默认使用当前执行命令的目录 作为源码目录。|
|隐含行为|	指定生成器 (Generator)。	|当您没有使用 -G 选项时，CMake 会根据操作系统自动选择默认生成器，通常是 Unix Makefiles (Linux/macOS) 或 Visual Studio (Windows)。|

该-B标志指示 CMake 使用指定的相对路径作为构建过程中生成文件和存储构建产物的位置。如果省略该标志，则使用当前工作目录。通常认为将生成的文件放在源代码树中是一种不好的做法，即进行“源代码内”构建(生成的文件会“污染”源文件)。  
2)  构建阶段：`cmake --build build`

接下来，告诉 CMake 使用 构建项目 ，并传递与使用标志时相同的相对路径。`cmake --build-B`
```bash
cmake --build build
```

`CMake `会调用实际的底层构建工具（如 make 或 ninja）来执行编译和链接操作。命令部分作用解释cmake再次调用 CMake 程序。  
`--build`触发构建操作。这是 CMake 提供的、跨平台的构建接口。它会读取构建目录中的配置信息。    
`build`指定要构建的目录。CMake 会进入 build 目录，确定之前使用的是哪个生成器（比如 make），然后自动运行相应的构建命令（比如在后台执行 make）。  
**总结：** 此命令会使用上一步配置中确定的构建系统，编译您的源代码并生成最终的可执行文件或库。
**为什么使用 cmake --build？**使用 cmake --build 而不是直接调用 make 或 ninja 的主要优势是 跨平台一致性。无论您在 Linux (使用 make) 还是 Windows (使用 MSBuild)，您都可以使用同一条命令进行构建，而无需记住底层构建工具的具体名称或参数。

## 练习1

```cpp
#include <iostream>
#include <string>

// TODO8: Include the MathFunctions header

int main(int argc, char* argv[])
{
  if (argc < 2) {
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  double const inputValue = std::stod(argv[1]);

  // TODO9: Use the mathfunctions::sqrt function
  // calculate square root
  double const outputValue = std::sqrt(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
}
```
```bash
  GNU nano 6.2                    ../CMakeLists.txt                             
# TODO1: Set the minimum required version of CMake to be 3.23
cmake_minimum_required(VERSION 3.23)
# TODO2: Create a project named Tutorial
project(Tutorial)
# TODO3: Add an executable target called Tutorial to the project
add_executable(Tutorial)
# TODO4: Add the Tutorial/Tutorial.cxx source file to the Tutorial target
target_sources(Tutorial
PRIVATE
Tutorial/Tutorial.cxx)
# TODO7: Add the MathFunctions library as a linked dependency
#        to the Tutorial target

# TODO11: Add the Tutorial subdirectory to the project

# TODO5: Add a library target called MathFunctions to the project

# TODO6: Add the source and header file located in Step1/MathFunctions to the
#        MathFunctions target, note that the intended way to include the
#        MathFunctions header is:

```

```bash
jorben@jorben-desktop:~/桌面/cmake-4.2.0-rc3-tutorial-source/Step1$ cmake -B build
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (1.7s)
-- Generating done (0.0s)
-- Build files have been written to: /home/jorben/桌面/cmake-4.2.0-rc3-tutorial-source/Step1/build
jorben@jorben-desktop:~/桌面/cmake-4.2.0-rc3-tutorial-source/Step1$ cmake --build build
[ 50%] Building CXX object CMakeFiles/Tutorial.dir/Tutorial/Tutorial.cxx.o
[100%] Linking CXX executable Tutorial
[100%] Built target Tutorial
```

```bash
jorben@jorben-desktop:~/桌面/cmake-4.2.0-rc3-tutorial-source/Step1/build$ ./Tutorial 23
The square root of 23 is 4.79583

```

## 构建一个库

`add_library(MyLibrary)` 
```
target_sources(MyLibrary
  PRIVATE
    library_implementation.cxx

  PUBLIC
    FILE_SET myHeaders
    TYPE HEADERS
    BASE_DIRS
      include
    FILES
      include/library_header.h
)
```
这里的信息量很大，所以我们可以使用一些简便方法。值得注意的是，如果名称FILE_SET与类型相同，则无需提供该TYPE字段。
`cmake -P CMakeLists.txt`脚本模式运行

`cmake --build build --clean-first  `用于编译前删除之前配置（会自动配置）

`add_subdirectory` 添加子目录(调用子目录的CML)

`set(var "World!")`

`message("Hello ${var}")`


```
foreach(stooge IN LISTS stooges)
  message("Hello, ${stooge}")
endforeach()
```

```
macro(MyMacro MacroArgument)
  message("${MacroArgument}\n\t\tFrom Macro")
endmacro()

function(MyFunc FuncArgument)
  MyMacro("${FuncArgument}\n\tFrom Function")
endfunction()

MyFunc("From TopLevel")
```
输出
```
$cmake -P CMakeLists.txt
From TopLevel
      From Function
              From Macro
```

然而，在某些情况下，这是必要的。包含空格的字符串需要用双引号括起来，否则 CMake 会将其视为列表；它会将元素用分号连接起来。反之亦然，当使用花括号扩展列表时，如果想要 保留分号，则必须将其放在引号内。否则，CMake 会将列表项展开成以空格分隔的字符串。
${list_value}  ${${list_value}}  "${${list_value}}" 时不一样的 前者正常情况是打开列表，在引用的条件下是列表名字 而中间的在引用中是打开列表 最后的是在引用中打开列表但保留分号。
`include()`
```
function(FilterFoo OutVar)

  foreach(item IN LISTS ARGN)
    if(item MATCHES Foo)
      list(APPEND ${OutVar} ${item})
    endif()
  endforeach()

  set(${OutVar} ${${OutVar}} PARENT_SCOPE)
endfunction()
```
ARGN: 是 CMake 函数中的一个特殊变量，代表了所有剩余的参数（即除了 OutVar 之外的所有参数）。
if(item MATCHES Foo): 对当前元素 ${item} 执行一个正则表达式匹配。

MATCHES Foo: 如果 ${item} 包含子字符串 "Foo"，则条件为真。

## 配置和缓存变量
-D由旗帜和创建的名称option()这些并非普通变量，而是缓存变量。缓存变量是全局可见且具有粘性（即初始设置后难以更改）的变量。事实上，它们的粘性非常强，以至于在项目模式下，CMake 会在多个配置之间保存和恢复缓存变量。如果一个缓存变量被设置一次，它将一直保留，直到另一个-D 标志位覆盖该已保存的变量为止。  

option 中间是描述性文字
```bash
option(COMPRESSION_SOFTWARE_USE_ZLIB "Support Zlib compression" ON)
option(COMPRESSION_SOFTWARE_USE_ZSTD "Support Zstd compression" ON)

if(COMPRESSION_SOFTWARE_USE_ZLIB)
  # Same as before
# ...
$cmake -B build \
    -DCOMPRESSION_SOFTWARE_USE_ZLIB=OFF
...
I will use Zstd!
```
set()也可用于操作缓存变量，但不会更改已创建的变量。
```bash
set(StickyCacheVariable "I will not change" CACHE STRING "")
set(StickyCacheVariable "Overwrite StickyCache" CACHE STRING "")

message("StickyCacheVariable: ${StickyCacheVariable}")
```
" CACHE STRING "
- CACHE 关键标志。这告诉 CMake 必须将这个变量存储到项目构建目录下的 CMakeCache.txt 文件中。一旦缓存被写入，这个变量的值就会被保留，即使在后续配置中移除这条 set 命令，除非用户手动更改或清除缓存。
- STRING	变量的类型。这是存储在缓存文件中的类型提示。常见的类型包括 STRING (字符串)、BOOL (布尔值) 和 FILEPATH (文件路径)。

缓存变量通常无法更改，但它们可以被普通变量覆盖。我们可以通过以下方式观察到这一点：set()将一个变量设置为与缓存变量同名，然后使用unset()移除正常变量
```bash
set(ShadowVariable "In the shadows" CACHE STRING "")
set(ShadowVariable "Hiding the cache variable")
message("ShadowVariable: ${ShadowVariable}")

unset(ShadowVariable)
message("ShadowVariable: ${ShadowVariable}")

$cmake -P ShadowVariable.cmake
ShadowVariable: Hiding the cache variable
ShadowVariable: In the shadows
```

### CMAKE 变量


### CMakePresets.json
调用 CMake 时，以前我们会这样做：

cmake -B build -DEXAMPLE_FOO=Bar -DEXAMPLE_QUX=Baz
现在我们可以使用预设值了：

cmake -B build --preset example-preset
```
{
  "version": 4,
  "configurePresets": [
    {
      "name": "example-preset",
      "cacheVariables": {
        "EXAMPLE_FOO": "Bar",
        "EXAMPLE_QUX": "Baz"
      }
    }
  ]
}
```

```
{
  "version": 4,
  "configurePresets": [
    {
      "name": "tutorial",
      "displayName": "Tutorial Preset",
      "description": "Preset to use with the tutorial",
      "binaryDir": "${sourceDir}/build",
      "cacheVariables": {
        "CMAKE_CXX_STANDARD": "20"
      }
    }
  ]
}

```

sourceDir 代表项目的源代码根目录

## 目标命令详解
`set_target_propertie`

`get_target_property`

`target_compile_features(MyApp PRIVATE cxx_std_20)`将最低语言标准描述为目标属性
`target_compile_definitions()`该命令将编译定义描述为目标属性
我们既不需要也不希望为-D用以下方式描述的编译定义添加前缀。target_compile_definitions()CMake 会根据当前编译器确定正确的标志。
()
`target_compile_options(MyApp PRIVATE -Wall -Werror)`传编译器flag

`target_link_options(MyApp PRIVATE -T LinksScript.ld)`传连接器flag  

| 开关      | 给谁用 | 含义  |
|---|---|---|
| `-Wall`  | 编译器 | 打开“所有”常规警告（未用变量、类型不匹配…）  |
| `-Werror` | 编译器 | 把上述警告当成错误，只要报警就**编译失败**  |
| `-T LinkScript.ld` | 链接器 | 告诉链接器“别按默认规则排布段，按我写的脚本排布”.text/.data等，嵌入式/内核常用 |
