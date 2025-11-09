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
- shell 传递参数用 $n n=0 1 2 3...
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
-Bash 支持关联数组，可以使用任意的字符串、或者整数作为下标来访问数组元素。  
关联数组使用 declare 命令来声明，语法格式如下：
```bash
declare -A site=(["google"]="www.google.com" ["runoob"]="www.runoob.com" ["taobao"]="www.taobao.com")
#也可以声明后再设键和值
declare -A site
site["google"]="www.google.com"
site["runoob"]="www.runoob.com"
site["taobao"]="www.taobao.com"
```
printf
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
*控制流程:*
-if
```bash
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```