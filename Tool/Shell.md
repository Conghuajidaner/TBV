# [Shell学习](https://www.runoob.com/linux/linux-shell.html)

> Shel 是一个C语言的程序，用户使用Linux的桥梁，既是一种命令语言也是一种程序设计语言。同时Shell作为一个应用程序，用户通过Shell去访问操作系统的服务。

并不需要一个复杂的环境配置，只需要一个编辑器以及一个解释器，比较通用的是bash，同时也是大部分Linux的默认shell.

```shell

#! /bin/bash # 作为脚本的开始，告诉系统后面指定的路径就是解释脚本的shell的程序
echo "Hello World !"
```

```.sh
chmod +x ./test.sh #脚本获得执行权限
./test.sh #脚本执行，运行二进制脚本都要写成./的形式
```



## 变量  

> 变量与等号之间不能有空格，变量可以被重新定义，同时有以下的命名规则

- 命名只能私用字母数字下划线
- 中间不能有空格
- 不能使用关键字

除了显示的命名，还可在**循环里面去给变量赋值**，例：

```shell
for file in `ls /etc`
for file in $(ls /etc)
```

**使用变量**就是在变量的前面加一个美元符号, 花括号并不是必须的，但是有效的帮助解释器识别变量边界，例：

```shell
your_name="qinjx"
echo $your_name
echo ${your_name}
```

```shell
# 这里的Ada Coffe Action Java 相当于一个字符数组被遍历
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
```

**只读变量**需要使用`readonly`,只读变量的数值不能被改变

**删除变量**使用**unset**命令，`echo`被删除的变量是一个空值

> 变量类型分为三种：局部变量，环境变量，shell变量

1. 局部变量是在脚本或者命令中定义，只在当前的shell中有效
2. 所有的程序包括shell都可以访问环境变量，必要的时候shell脚本也可以定义环境变量
3. shell变量是程序设置的特殊变量，一部分环境变量，一部分局部变量

## 字符串

> 字符串是表常用的数据类型，可以是单引号，双引号，甚至不用引号

- 单引号的字符串并不会被解释器解释
- 单引号字符串不能出现单独的单引号（即使是转移符号也不行），但是可以成对出现，作为字符串拼接
- 双引号里面可以有变量
- 双引号可以使用转义字符
- **${#string}**获取字符串长度

```shell
your_name='runoob'
str="Hello, I know you are \"$your_name\"! \n"
echo -e $str
# Hello, I know you are "runoob"! 
```

**字符串拼接**

```shell
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# hello, runoob ! hello, runoob !

# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
# hello, runoob ! hello, ${your_name} !

string="abcd"
echo ${#string} #输出 4
```

**提取子字符串**下标从0开始

```shell
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

**查找子字符串**

```shell
# 查找i，o的第一个位置
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```

