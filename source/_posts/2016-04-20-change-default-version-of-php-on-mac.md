---
layout: post
title: 如何更改mac os的默认php版本
date: 2016-04-20
---
使用mac的童鞋都知道，mac自带有php，apache等，使得php开发环境的搭建异常简单，但是mac自带的php版本普遍老旧，而且许多扩展都没有编译进去，例如mcrypt等。因此在本地调试时，也就导致当我们需要某项php扩展支持时，还需重新编译，非常麻烦（因为要用到许多shell命令，更可怕的是一旦安装时出现其他依赖未安装问题，那真是够你折腾一阵了）。而我们在mac上安装的lamp以及mamp等集成开发环境，大多有众多的php版本可选，且开启关闭扩展都很简单。所以，如果能把mac上的默认php修改为MAMP等扩展环境中的，那么这些问题就so easy了。
下面我们就介绍如何操作。
1. 首先，打开terminal。
然后在用户home目录下执行vi .bash_profile,这个文件也有可能没有，如果没有就新建一个（在用户home目录下）。
2. 在vi中打开或新建好该文件后在文件末尾添加
```
export PATH=/Applications/MAMP/bin/php/php5.4.10/bin:$PATH
```
==注：“=”号之后，“：”号之前的路径就是你要修改成的php的路径==，我这里选取的是MAMP中自带的5.4版本

3. 保存后就大功告成了，接着我们验证一下，另开terminal，输入which PHP,有没有发现默认php的路径已经修改好了呢。接下来就可已正常使用了，如果还不行，重启电脑就可以了。