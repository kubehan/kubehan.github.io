---
title: mail 命令发送附件 Linux 亲测简单实用
author: Kubehan
type: post
date: 2020-03-16T03:47:27+08:00
url: /990.html
views:
  - 818
  - 818
post_views_count:
  - 0
  - 0
categories:
  - Linux运维

---
在网上看了好多教程，太乱了、发送附还要安装这个那个的安装包，太烦了

事实上mail命令就可以实现啊 只需要加-a参数接文件就ok了嘛，哪有那么麻烦

echo -e “邮件正文部分” | mail -a 附件路径 -s 主题&nbsp;&nbsp;&nbsp;&nbsp;邮箱地址

这样就可以发送了

下面是邮箱配置脚本

<pre class="brush:bash;toolbar:false">#!/bin/bash
#&nbsp;@Author:&nbsp;HanWei
#&nbsp;@Date:&nbsp;&nbsp;&nbsp;2019-12-05&nbsp;09:27:36
#&nbsp;@Last&nbsp;Modified&nbsp;by:&nbsp;&nbsp;&nbsp;HanWei
#&nbsp;@Last&nbsp;Modified&nbsp;time:&nbsp;2019-12-06&nbsp;10:49:45
#&nbsp;@E-mail:&nbsp;han_wei_95@163.com
#安装对应的数字证书：这里请求安装网易163邮箱数字证书
mkdir&nbsp;-p&nbsp;/root/.certs/
#向163请求证书
echo&nbsp;-n&nbsp;|&nbsp;openssl&nbsp;s_client&nbsp;-connect&nbsp;smtp.163.com:465&nbsp;|&nbsp;sed&nbsp;-ne&nbsp;&#39;/-BEGIN&nbsp;CERTIFICATE-/,/-END&nbsp;CERTIFICATE-/p&#39;&nbsp;&gt;&nbsp;~/.certs/163.crt
#增加一个证书到证书数据库中
certutil&nbsp;-A&nbsp;-n&nbsp;"GeoTrust&nbsp;SSL&nbsp;CA"&nbsp;-t&nbsp;"C,,"&nbsp;-d&nbsp;~/.certs&nbsp;-i&nbsp;~/.certs/163.crt
#再增加一个证书到证书数据库中
certutil&nbsp;-A&nbsp;-n&nbsp;"GeoTrust&nbsp;Global&nbsp;CA"&nbsp;-t&nbsp;"C,,"&nbsp;-d&nbsp;~/.certs&nbsp;-i&nbsp;~/.certs/163.crt
certutil&nbsp;-L&nbsp;-d&nbsp;/root/.certs
cd&nbsp;/root/.certs/
certutil&nbsp;-A&nbsp;-n&nbsp;"GeoTrust&nbsp;SSL&nbsp;CA&nbsp;-&nbsp;G3"&nbsp;-t&nbsp;"Pu,Pu,Pu"&nbsp;-d&nbsp;./&nbsp;-i&nbsp;163.crt
#配置客户端信息
echo&nbsp;"
set&nbsp;from=95@163.com&nbsp;&nbsp;#开启SMTP服务的邮箱
set&nbsp;smtp=smtps://smtp.163.com:465
set&nbsp;smtp-auth-user=95@163.com
set&nbsp;smtp-auth-password=h63com&nbsp;&nbsp;&nbsp;&nbsp;#邮箱的授权码
set&nbsp;smtp-auth=login
set&nbsp;ssl-verify=ignore
set&nbsp;nss-config-dir=/root/.certs&nbsp;&nbsp;&nbsp;#证书所在目录
"&nbsp;&gt;&gt;&nbsp;/etc/mail.rc
#测试成功信息
echo&nbsp;"恭喜您！您已成功配置邮箱，现在可以正常收件！"&nbsp;|&nbsp;mail&nbsp;-s&nbsp;"PHP党建平台邮箱配置通知"&nbsp;han_wei_95@163.com</pre>