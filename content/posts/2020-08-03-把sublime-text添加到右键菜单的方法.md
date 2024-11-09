---
title: 把Sublime Text添加到右键菜单的方法
author: Kubehan
type: post
date: 2020-08-03T02:16:24+08:00
url: /2662.html
Baidusubmit:
  - 1
views:
  - 740
post_img:
  - 'true'
hot:
  - 'true'
zm_like:
  - 1
post_style:
  - sidebar
cao_vip_rate:
  - 1
categories:
  - Linux运维

---
前一段重新安装了Sublime Text3，不过一直不在右键菜单中，所以决定添加，有如下2种方法。

方法一（推荐）、

把以下代码，复制到SublimeText3的安装目录，然后重命名为：sublime_addright.inf，然后右击安装就可以了。

PS：重命名文件之前，需要先在工具--文件夹选项，查看中，把隐藏已知文件类型的扩展名前边的复选框不勾选。

<pre><code class="language-bash">[Version]
Signature="$Windows NT$"

[DefaultInstall]
AddReg=SublimeText3

[SublimeText3]
hkcr,"*\\shell\\SublimeText3",,,"用 SublimeText3 打开"
hkcr,"*\\shell\\SublimeText3\\command",,,"""%1%\sublime_text.exe"" ""%%1"" %%*"
hkcr,"Directory\shell\SublimeText3",,,"用 SublimeText3 打开"
hkcr,"*\\shell\\SublimeText3","Icon",0x20000,"%1%\sublime_text.exe, 0"
hkcr,"Directory\shell\SublimeText3\command",,,"""%1%\sublime_text.exe"" ""%%1"""</code></pre>

看回复有不少人，询问右键乱码问题：

解决方案：用记事本打开sublime_addright.inf，然后点击菜单，另存为；文件名和路径不变，设置编码格式为：ANSI ，然后重新安装就可以了

方法二、

把以下代码，复制到SublimeText3的安装目录，然后重命名为：sublime_addright.reg，然后双击就可以了。

PS:需要把里边的Sublime的安装目录，替换成实际的Sublime安装目录。

<pre><code class="language-bash">Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\*\shell\SublimeText3]
@="用 SublimeText3 打开"
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"

[HKEY_CLASSES_ROOT\*\shell\SublimeText3\command]
@="D:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"

[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3]
@="用 SublimeText3 打开"
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"

[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3\command]
@="D:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"

 ```

最后，附一个删除右键菜单的脚本吧。

把以下代码，复制到SublimeText3的安装目录，然后重命名为：sublime_delright.reg，然后双击就可以了。```bash
```bash
Windows Registry Editor Version 5.00
[-HKEY_CLASSES_ROOT\*\shell\SublimeText3]
[-HKEY_CLASSES_ROOT\Directory\shell\SublimeText3]</code></pre>