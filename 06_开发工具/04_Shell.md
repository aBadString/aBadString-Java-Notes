<h1 align="center">Shell</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. 变量](#1-变量)
- [2. 字符串](#2-字符串)
- [3. 数组](#3-数组)
- [4. 注释](#4-注释)
- [5. 命令行参数](#5-命令行参数)
- [6. 基本运算符](#6-基本运算符)

<!-- /code_chunk_output -->

Bash: Bourne Again Shell

# 1. 变量

**定义变量**  
定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：
```shell
your_name="runoob.com"
```
注意，变量名和等号之间不能有空格。

同时，变量名的命名须遵循如下规则：  
- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
- 中间不能有空格，可以使用下划线（_）。
- 不能使用标点符号。
- 不能使用bash里的关键字（可用help命令查看保留关键字）。

**使用变量**  
使用一个定义过的变量，只要在变量名前面加美元符号即可。  
变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界。


```shell
#!/bin/bash

# 使用变量 $变量名 / ${变量名}
your_name="songwan"
echo $your_name
echo ${your_name}

# 变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done

# 变量可以被重新赋值
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name

# 只读变量
myUrl="https://www.google.com"
readonly myUrl
#myUrl="https://www.runoob.com"

# 删除变量 unset 
# 变量被删除后不能再次使用。unset 命令不能删除只读变量。
runoob="https://www.runoob.com"
unset runoob
echo $runoob

#unset myUrl
```

# 2. 字符串

**单引号字符串：**  
单引号里的任何字符都会**原样输出**，单引号字符串中的变量是无效的；  
单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

**双引号的字符串：**  
双引号里可以有变量  
双引号里可以出现转义字符 \n \"

```shell
your_name="runoob"
# 使用双引号拼接
# "hello, " + $your_name + " !"
# 双引号内 $ 会被转义
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# hello, runoob ! hello, runoob !

# 使用单引号拼接 'hello, ' + $your_name + ' !'
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
# hello, runoob ! hello, ${your_name} !
```

字符串长度：`${#your_name}`

截取自字符串：`${your_name:1:4}` 从下标为 1 的位置开始(含该位置)往后取 4 个字符 (下标从0开始)


# 3. 数组

数组名=(值1 值2 ... 值n)
```shell
array_name=(value0 value1 value2 value3)

# 使用 @ 或 * 符号可以获取数组中的所有元素
echo ${array_name[2]}
echo ${array_name[@]}
echo ${array_name[*]}
# value2
# value0 value1 value2 value3
# value0 value1 value2 value3

# 取得数组元素的个数
length=${#array_name[@]}
echo $length
# 或者
length=${#array_name[*]}
echo $length

# 取得数组单个元素的长度
length=${#array_name[1]}
echo $length
```

# 4. 注释

```shell
# 单行注释

:<<EOF
多行注释
多行注释
EOF

# EOF 也可以使用其他符号:
:<<'
多行注释
多行注释
'

:<<!
多行注释
多行注释
!
```

# 5. 命令行参数

> https://www.runoob.com/linux/linux-shell-passing-arguments.html

```shell
echo "脚本文件名："$0
echo "第一个参数："$1
echo "第二个参数："$1
echo "第三个参数："$3
echo "参数个数为："$#
echo "脚本运行的当前进程ID号："$$
# 脚本文件名：04_argument.sh
# 第一个参数：1
# 第二个参数：1
# 第三个参数：3
# 参数个数为：3
```

**$* 与 $@ 区别：**  
相同点：都是引用所有参数。  
不同点：只有在【双引号】中体现出来。  
        假设在脚本运行时写了三个参数 1、2、3，，  
        则 "$*" 等价于 "1 2 3"（传递了一个参数）(特殊)，  
        而 "$@" 等价于 "1" "2" "3"（传递了三个参数）。  
       没有【双引号】时，  
        $* 和 $@ 都是传递了三个参数  

# 6. 基本运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。  
expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

> https://www.runoob.com/linux/linux-shell-basic-operators.html
