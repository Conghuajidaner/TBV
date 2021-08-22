# [LLDB学习](https://lldb.llvm.org)

> MacOs提供了一套对于进程的保护机制，但是这个机制很有可能会影响你对于源代码的调试，如果你遇到了这个问题请去关闭Rootless

## 命令格式

相对于Gdb来说，lldb的命令更加标准化

```
<noun> <verb> [-option [[opyion-value]]] [arguement[arguement...]]
```

- 命令行的解析是在命令执行之前就已经完成的，因此对于所有的命令都是统一的。基本命令的语法比较简单，参数选项都是空格分离，是用双引号来保护参数中的空格
- 选项可以放在命令的任何位置，但是如果参数用`-`开头，必须添加终止选项来告诉lldb，已经完成了当前的命令选项

1. 要在LLDB中设置相同的文件和行断点下面两种方式

```
(lldb) breakpoint set --file foo.c --line 12
(lldb) breakpoint set -f foo.c -l 12
```

2. 要在LLDB中为foo函数上设置断点
3. 也可以一组函数设置断点
4. 设置选择器
5. --shlib

## 将程序加载到LLDB

1. 我们可以在程序运行之前加载以及运行时候去通过文件加载
2. 同时我们可以通过进程的pid去指定加载

## 设置断点

`breakpoint list`可以查看我们的断点信息

## 设置观察断点

除了设置断点以外，我们还可以使用帮助监视点去查看所有点的操作

```
watch set var global
```

## 启动或附加到您的程序

## 控制您的程序

```
thread continue
```

