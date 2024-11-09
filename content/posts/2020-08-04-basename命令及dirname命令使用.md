---
title: basename命令及dirname命令使用
author: Kubehan
type: post
date: 2020-08-04T07:57:43+08:00
url: /2668.html
Baidusubmit:
  - 1
views:
  - 834
cat_top:
  - 'true'
post_img:
  - 'true'
hot:
  - 'true'
zm_like:
  - 2
baidu_record:
  - 1
categories:
  - Linux运维

---
#### dirname命令

dirname命令去除文件名中的非目录部分，仅显示与目录有关的内容。dirname命令读取指定路径名保留最后一个/及其后面的字符，删除其他部分，并写结果到标准输出。如果最后一个/后无字符，dirname 命令使用倒数第二个/，并忽略其后的所有字符。dirname 和 basename 通常在 shell 内部命令替换使用，以指定一个与指定输入文件名略有差异的输出文件名。  
案例：

<pre><code class="language-bash">dirname //
结果为 /
dirname /a/b/
结果为：/a
dirname a
结果为 .
dirname a/b
结果为路径名 a</code></pre>

一句话说dirname是过滤目录的命令

#### basename命令

basename命令用于打印目录或者文件的基本名称。basename和dirname命令通常用于shell脚本中的命令替换来指定和指定的输入文件名称有所差异的输出文件名称。

案例：  
1、要显示一个shell变量的基本名称，请输入：

<pre><code class="language-bash">basename $WORKFILE</code></pre>

此命令显示指定给shell变量WORKFILE的值的基本名称。如果WORKFILE变量的值是/home/jim/program.c文件，则此命令显示program.c。

要构造一个和另一个文件名称相同（除了后缀）的文件名称，请输入：

<pre><code class="language-bash">OFILE=`basename $1 .c`.o</code></pre>

此命令指定给 OFILE 文件第一个位置上的参数（$1）的值，但它的 .c 后缀更改至 .o。如果 $1 是 /home/jim/program.c 文件，则 OFILE 成为 program.o。因为 program.o 仅是一个基本文件名称，它标识在当前目录中的文件。