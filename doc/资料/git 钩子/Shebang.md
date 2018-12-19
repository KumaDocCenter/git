# Shebang

## 释义

原文：  [Shebang（Unix） - 维基百科](https://en.wikipedia.org/wiki/Shebang_(Unix) )

在计算中，shebang是由字符#号符号和感叹号（#!）组成的字符序列。在脚本的开头。它也被称为 **sha-bang**, **hashbang**, **pound-bang**, 或 **hash-pling**.

在类Unix的操作系统中，当带有shebang的文本文件被当作可执行文件使用时，程序加载器将文件初始行的其余部分解析为解释器指令；执行指定的解释器程序，将最初使用的路径作为参数传递给它当试图运行脚本时，使程序可以使用文件作为输入数据。 

## 语法

shebang 翻译指令的形式如下 

```
#!interpreter [optional-arg]
```

其中`interpreter`是可执行程序的绝对路径。 

`[optional-arg]`，可选参数是表示单个参数的字符串。 



## LINUX上的SHEBANG符号(#!)

原文：  [LINUX上的SHEBANG符号(#!)](http://smilejay.com/2012/03/linux_shebang/)

Shebang这个符号通常在Unix系统的脚本中第一行开头中写到，它指明了执行这个脚本文件的解释程序。

1. 如果脚本文件中没有#!这一行，那么它执行时会默认用当前Shell去解释这个脚本(即：$SHELL环境变量）。

2. 如果#!之后的解释程序是一个可执行文件，那么执行这个脚本时，它就会把文件名及其参数一起作为参数传给那个解释程序去执行。

3. 如果#!指定的解释程序没有可执行权限，则会报错“bad interpreter: Permission denied”。

4. 如果#!指定的解释程序不是一个可执行文件，那么指定的解释程序会被忽略，转而交给当前的SHELL去执行这个脚本。

5. 如果#!指定的解释程序不存在，那么会报错“bad interpreter: No such file or directory”。

   注意：#!之后的解释程序，需要写其绝对路径（如：#!/bin/bash），它是不会自动到$PATH中寻找解释器的。

6. 当然，如果你使用"bash test.sh"这样的命令来执行脚本，那么#!这一行将会被忽略掉，解释器当然是用命令行中显式指定的bash。



## 参考阅读

 [Shebang（Unix） - 维基百科](https://en.wikipedia.org/wiki/Shebang_(Unix) )

 [一分钟看懂头部 shell #!/usr/bin命令](https://www.jianshu.com/p/41efd651c227)

 [LINUX上的SHEBANG符号(#!)](http://smilejay.com/2012/03/linux_shebang/)