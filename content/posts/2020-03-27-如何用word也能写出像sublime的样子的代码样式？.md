---
title: 如何用word也能写出像Sublime的样子的代码样式？
author: Kubehan
type: post
date: 2020-03-27T09:22:33+08:00
url: /2015.html
views:
  - 888
  - 888
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032720_0922_wordSub1.png
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032720_0922_wordSub1.png
cms_top:
  - 'true'
  - 'true'
zm_like:
  - 6
  - 6
classic-editor-remember:
  - classic-editor
  - classic-editor
categories:
  - Linux运维

---
先来比较一下

Sublime下的shell样式：  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032720_0922_wordSub1.png" alt="" /> 

再来看看word里面的样式

<pre style="background: #333333; text-align: left;"><span style="color: skyblue; font-family: Courier New; font-size: 10pt;">#!/bin/bash</span>
<span style="color: skyblue; font-size: 10pt;"><span style="font-family: 等线;">#随机修改密码</span></span>
<span style="color: skyblue; font-family: Courier New; font-size: 10pt;">#################################################    </span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">date=<span style="color: #ffa0a0;">`date`</span></span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">hostname=<span style="color: #ffa0a0;">`hostname`</span></span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">passwd=<span style="color: #ffa0a0;">`openssl rand -base64 16`</span></span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">echo $passwd</span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">echo $passwd &gt;&gt; passwd.txt</span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">chattr +a passwd.txt</span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">user=$1</span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">echo ${passwd} | passwd --stdin $user</span>
<span style="color: white; font-size: 10pt;"><span style="font-family: Courier New;">echo <span style="color: #ffa0a0;">"passwd is ${passwd} for $user"<span style="color: white;"> | mail -s <span style="color: #ffa0a0;">"$date $hostname </span></span></span></span><span style="font-family: 等线;">密码修改通知</span><span style="color: #ffa0a0; font-family: Courier New;"> "<span style="color: white;"> han@163.com</span></span></span>
<span style="color: skyblue; font-size: 10pt;"><span style="font-family: Courier New;">#</span><span style="font-family: 等线;">还原密码</span></span>
<span style="color: white; font-family: Courier New; font-size: 10pt;">passwd=<span style="color: #ffa0a0;">`hostname`
</span></span><span style="color: white; font-family: Courier New; font-size: 10pt;">echo <span style="color: #ffa0a0;">`hostnaem`<span style="color: white;"> | openssl base64 | passwd --stdin
</span></span></span></pre>

实现方法很简单

找到插入\---<span style="font-family: Wingdings;">à</span>获取加载项—-> 安装Easy Code Formatter就能实现了

先进到设置里面过一遍设置好自己的主题模式

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032720_0922_wordSub2.png" alt="" /> 

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032720_0922_wordSub3.png" alt="" /> 

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032720_0922_wordSub4.png" alt="" /> 

写完代码后点击<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032720_0922_wordSub5.png" alt="" /> 就可以实现