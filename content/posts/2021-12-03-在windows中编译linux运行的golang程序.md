---
title: 在Windows中编译Linux运行的Golang程序
author: Kubehan
type: post
date: 2021-12-03T08:02:33+08:00
url: /3222.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 823
categories:
  - 编程语言

---
打开CMD，先修改Go环境参数，然后再编译。编译结束恢复为windows的环境参数。  
注意：不知道为什么，在VsCode的Terminal中操作时会失败，但是在cmd.exe中是可以的。

修改go环境参数

<pre><code class="language-Go">SET CGO_ENABLED=0
SET GOOS=linux
SET GOARCH=amd64</code></pre>

设置完之后，可以查看一下设置是否生效：

<pre><code class="language-Go">go env CGO_ENABLED
go env GOOS
go env GOARCH</code></pre>

将环境参数改回windows  
也可不改回，取决于具体需要

<pre><code class="language-Go">SET CGO_ENABLED=1
SET GOOS=windows
SET GOARCH=amd64</code></pre>