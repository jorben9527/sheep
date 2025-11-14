# Shell 基础学习
**在这里我会随手记录我所学到的东西**
- 声明 整型变量 `declare -i my_integer=42`
- 数组变量 `my_array=(1 2 3 4 5)`
- 双引号里可以有变量  双引号里可以出现转义字符
```Bash
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1

# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```
- 输出字符串长度 以及从第几个字符输出几个字符
```
string="abcd"
echo ${#string}   # 输出 4

string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```
## shell 传递参数用 $n n=0 1 2 3...
  n=0 这个参数是文件名 其他参数是依次输入的内容如
```bash
#!/bin/bash

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```
终端：
```bash
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

- 几种特殊的参数
```bash
#!/bin/bash

echo "Shell 传递参数实例！";
echo "第一个参数为：$1";

echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```
终端
```bash
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3

```
## Bash 支持关联数组，可以使用任意的字符串、或者整数作为下标来访问数组元素。  
关联数组使用 declare 命令来声明，语法格式如下：
```bash
declare -A site=(["google"]="www.google.com" ["runoob"]="www.runoob.com" ["taobao"]="www.taobao.com")
#也可以声明后再设键和值
declare -A site
site["google"]="www.google.com"
site["runoob"]="www.runoob.com"
site["taobao"]="www.taobao.com"
```
## test 判断工具  *注意方括号内必须有空格*
|操作符|描述|示例|
|---|---|---|
|-e|文件是否存在|	[ -e file.txt ]|
|-f	|是普通文件|[ -f /path/to/file ]|
|-d|是目录|[ -d /path/to/dir ]|
|-r|可读|[ -r file.txt ]|
|-w|可写|[ -w file.txt ]|
|-x|可执行|	[ -x script.sh ]|
|-s|文件大小 >0|[ -s logfile ]|
|-L|是符号链接|[ -L symlink ]|


数值比较 *操作符两边的空格不可缺*
|操作符|	描述|	示例|
|---|---|---|
|-eq	|等于	|[ "$a" -eq "$b" ]|
|-ne	|不等于|	[ "$a" -ne "$b" ]|
|-gt	|大于	|[ "$a" -gt "$b" ]|
|-ge	|大于或等于\	[ "$a" -ge "$b" ]|
|-lt	|小于	[ "$a" -lt "$b" ]|
|-le	|小于或等于	|[ "$a" -le "$b" ]|


- Bash 提供了更强大的测试语法： 

双括号 [[ ]]  
支持模式匹配：[[ "$var" == *.txt ]]  
支持正则表达式：[[ "$var" =~ ^[0-9]+$ ]]  

更安全的字符串处理  
算术比较 (( ))  
专为数值比较设计：(( a > b ))  
支持更复杂的算术表达式  

- 我发现大多时候变量引用都要加""
如果你的变量值中包含空格、制表符、换行符等空白字符，不加双引号会导致 Shell 将变量值拆分成多个独立的词（Word Splitting），并将它们作为不同的参数对待。

## printf：
- %s：字符串
- %d：十进制整数
- %f：浮点数
- %c：字符
- %x：十六进制数
- %o：八进制数
- %b：二进制数
- %e：科学计数法表示的浮点数
```bash
# 整数
printf "Decimal: %d\nHex: %x\nOctal: %o\n" 255 255 255

# 浮点数
printf "Float: %f\nScientific: %e\n" 3.14159 3.14159

# 字符串
printf "Name: %s\n" "Bob"

# 字符
printf "First letter: %c\n" "A"

```
### printf 的应用
- 输出表格
```bash
#!/bin/bash

# 表头
printf "%-15s %10s %10s %10s\n" "Item" "Quantity" "Price" "Total"

# 分隔线
printf "%-15s %10s %10s %10s\n" "---------------" "----------" "----------" "----------"

# 数据行
printf "%-15s %10d %10.2f %10.2f\n" "Notebook" 3 2.50 7.50
printf "%-15s %10d %10.2f %10.2f\n" "Pen" 5 1.20 6.00
printf "%-15s %10d %10.2f %10.2f\n" "Eraser" 2 0.50 1.00

# 总计行
printf "%-15s %10s %10s %10.2f\n" "" "" "Total:" 14.50
```
- 进度条
```bash
#!/bin/bash

for i in {1..20}; do
    printf "\rProgress: [%-20s] %d%%" $(printf "%${i}s" | tr ' ' '#') $((i*5))
    sleep 0.1
done
printf "\n"
```
- 颜色输出
```bash
#!/bin/bash

RED='\033[0;31m'
GREEN='\033[0;32m'
NC='\033[0m' # No Color

printf "${RED}Error:${NC} Something went wrong\n"
printf "${GREEN}Success:${NC} Operation completed\n"
```
- 格式化输出
```bash
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876
```

## 控制流程:
### if
```bash
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi

```

### for 
```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```
还可写成单行形式 *常用于命令行*
```bash
for var in item1 item2 ... itemN; do command1; command2… done;
```
### whlie 
*如果条件是 `true` 或者` : `就是无限循环*
```bash
while condition
do
    command
done
```

### until *与whlie 相反 条件为真才停止循环*
```bash
until condition
do
    command
done
```

### case ... esac
每个 case 分支用右圆括号开始，用两个分号 ;; 表示 break，即执行结束，跳出整个 case ... esac 语句，esac（就是 case 反过来）作为结束标记。  
如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。
```bash
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2)
    command1
    command2
    ...
    commandN
    ;;
esac
```
示例 *可以不是数字*
```bash
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```
多条件可以像下面一样`|`
```bash
case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
```
### break 
跳出循环
### continue 
跳出当前循环

### 疑惑的结构
- `$()` 
 这个不同于`$/${}`是命令替换 结构  
 就是它会执行括号内的命令，并将其标准输出 (stdout) 的内容作为整个表达式的值返回。
- `(( ))`运算扩展 可以 避免expr

- `$(()) `上面两个的结合

## Shell 函数
```bash
[ function ] funname [()]

{

    action;

    [return int;]

}
```
- 函数参数 直接在函数体写就行不需要定义 
*$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。*
```bash
#!/bin/bash
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```
- 返回值 
```bash
#!/bin/bash
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```
**函数返回值在调用该函数后通过 $? 来获得**
**return 语句只能返回一个介于 0 到 255 之间的整数，而两个输入数字的和可能超过这个范围。**


|参数处理	|说明|
|---|---|
|$#|	传递到脚本或函数的参数个数|
|$*	|以一个单字符串显示所有向脚本传递的参数|
|$$	|脚本运行的当前进程ID号|
|$!	|后台运行的最后一个进程的ID号|
|$@|	与$*相同，但是使用时加引号，并在引号中返回每个参数。|
|$-	|显示Shell使用的当前选项，与set命令功能相同。|
|$?	|显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。|

## 输出重定向

|命令	|说明|
|---|---|
|command > file|	将输出重定向到 file。|
|command < file|	将输入重定向到 file。|
|command >> file|	将输出以追加的方式重定向到 file。|
|n > file|	将文件描述符为 n 的文件重定向到 file。|
|n >> file|	将文件描述符为 n 的文件以追加的方式重定向到 file。|
|n >& m|	将输出文件 m 和 n 合并。|
|n <& m	|将输入文件 m 和 n 合并。|
|<< tag	|将开始标记 tag 和结束标记 tag 之间的内容作为输入。|