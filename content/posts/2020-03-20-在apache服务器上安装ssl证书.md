---
title: 在Apache服务器上安装SSL证书
author: Kubehan
type: post
date: 2020-03-20T07:43:47+08:00
url: /1068.html
wb_dl_type:
  - 1
  - 1
views:
  - 727
  - 727
wb_dl_mode:
  - 0
  - 0
wb_down_local_url:
  - 
  - 
wb_down_url_ct:
  - 
  - 
wb_down_url:
  - 
  - 
wb_down_pwd:
  - 
  - 
post_views_count:
  - 0
  - 0
bigger_cover:
  - https://www.kubehan.cn/wp-content/uploads/posterimg/poster-1068.png
categories:
  - Linux运维
tags:
  - Apache

---
在Apache服务器上安装SSL证书

本文介绍如何将从阿里云SSL证书控制台下载的证书安装到Apache服务器，从而使Apache服务器支持HTTPS安全访问。



前提条件

已安装OpenSSL。

本文档证书名称以domain name为示例，如证书文件名称为domain name\_public.crt，证书链文件名称为domain name\_chain.crt，证书秘钥文件名称为domain name.key。

申请证书时如果未选择系统自动创建CSR，证书下载压缩包中将不包含.key文件。

说明 .crt扩展名的证书文件采用Base64-encoded的PEM格式文本文件，可根据需要修改成.pem等扩展名。

操作步骤

登录阿里云SSL证书服务控制台。

单击已签发标签，定位到需要下载的证书，并单击证书卡片右下角的下载。

下载证书

在证书下载页面中定位到Apache服务器，单击右侧操作栏的下载。如果需要安装多个证书，需在SSL控制台下载这些证书。

Apache版证书

完成Apache版证书压缩包的下载，并保存到本地。

解压Apache证书。

解压后的文件夹中有3个文件：证书文件

证书文件（以.crt为后缀或文件类型）

证书链文件（以.crt为后缀或文件类型）

秘钥文件（以.key为后缀或文件类型）

在Apache安装目录中新建cert目录，并将下载的Apache证书、 证书链文件和秘钥文件拷贝到cert目录中。如果需要安装多个证书，需在Apache目录中新建对应数量的cert目录，用于存放不同的证书 。

说明 如果申请证书时选择了手动创建CSR文件，请将手动生成创建的秘钥文件拷贝到cert目录中并命名为domain name.key。

修改httpd.conf配置文件。

在Apache安装目录下，打开Apache/conf/httpd.conf文件，并找到以下参数，按照下文中注释内容进行配置。

#LoadModule ssl\_module modules/mod\_ssl.so&nbsp; #删除行首的配置语句注释符号“#”加载mod\_ssl.so模块启用SSL服务，Apache默认是不启用该模块的。如果找不到该配置，请重新编译mod\_ssl模块。

#Include conf/extra/httpd-ssl.conf&nbsp; #删除行首的配置语句注释符号“#”。&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

保存httpd.conf文件并退出。

修改httpd-ssl.conf配置文件。

打开Apache/conf/extra/httpd-ssl.conf文件并找到以下参数进行配置。 证书路径建议使用绝对路径。

说明 根据操作系统的不同，http-ssl.conf文件也可能存放在conf.d/ssl.conf目录中。

<VirtualHost *:443>&nbsp; &nbsp; &nbsp;

&nbsp; &nbsp; ServerName&nbsp; &nbsp;#修改为申请证书时绑定的域名www.YourDomainName1.com。&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;

&nbsp; &nbsp; DocumentRoot&nbsp; /data/www/hbappserver/public&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;

&nbsp; &nbsp; SSLEngine on&nbsp; &nbsp;# 添加SSL协议支持协议，去掉不安全的协议。

&nbsp; &nbsp; SSLProtocol all -SSLv2 -SSLv3

&nbsp; &nbsp; SSLCipherSuite HIGH:!RC4:!MD5:!aNULL:!eNULL:!NULL:!DH:!EDH:!EXP:+MEDIUM&nbsp; &nbsp;# 修改加密套件。

&nbsp; &nbsp; SSLHonorCipherOrder on

&nbsp; &nbsp; SSLCertificateFile cert/domain name1\_public.crt&nbsp; &nbsp;# 将domain name1\_public.crt替换成您证书文件名。

&nbsp; &nbsp; SSLCertificateKeyFile cert/domain name1.key&nbsp; &nbsp;# 将domain name1.key替换成您证书的秘钥文件名。

&nbsp; &nbsp; SSLCertificateChainFile cert/domain name1\_chain.crt&nbsp; # 将domain name1\_chain.crt替换成您证书的秘钥文件名；证书链开头如果有#字符，请删除。

</VirtualHost>



#如果证书包含多个域名，复制以上参数，并将ServerName替换成第二个域名。&nbsp;

<VirtualHost *:443>&nbsp; &nbsp; &nbsp;

&nbsp; &nbsp; ServerName&nbsp; &nbsp;#修改为申请证书时绑定的第二个域名www.YourDomainName2.com。&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;

&nbsp; &nbsp; DocumentRoot&nbsp; /data/www/hbappserver/public&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;

&nbsp; &nbsp; SSLEngine on&nbsp; &nbsp;# 添加SSL协议支持协议，去掉不安全的协议。

&nbsp; &nbsp; SSLProtocol all -SSLv2 -SSLv3

&nbsp; &nbsp; SSLCipherSuite HIGH:!RC4:!MD5:!aNULL:!eNULL:!NULL:!DH:!EDH:!EXP:+MEDIUM&nbsp; &nbsp;# 修改加密套件。

&nbsp; &nbsp; SSLHonorCipherOrder on

&nbsp; &nbsp; SSLCertificateFile cert/domain name2_public.crt&nbsp; &nbsp;# 将domain name2替换成您申请证书时的第二个域名。

&nbsp; &nbsp; SSLCertificateKeyFile cert/domain name2.key&nbsp; &nbsp;# 将domain name2替换成您申请证书时的第二个域名。

&nbsp; &nbsp; SSLCertificateChainFile cert/domain name2_chain.crt&nbsp; # 将domain name2替换成您申请证书时的第二个域名；证书链开头如果有#字符，请删除。

</VirtualHost>

说明 需注意您的浏览器版本是否支持SNI功能，如不支持多域名证书配置将无法生效。

保存 httpd-ssl.conf 文件并退出。

重启Apache服务器使SSL配置生效。

在Apache的bin目录下执行以下命令：

停止Apache服务。

apachectl -k stop

开启Apache服务。

apachectl -k start

可选： 设置Apache http自动跳转https。

在 httpd.conf 文件中的<VirtualHost *:80> </VirtualHost>中间，添加以下重定向代码。



RewriteEngine on

RewriteCond %{SERVER_PORT} !^443$

RewriteRule ^(.*)$ https://%{SERVER_NAME}$1 [L,R]

后续操作

证书安装完成后，可通过登录证书绑定域名的方式验证证书是否安装成功。

https://domain name&nbsp; &nbsp;#domain name替换成证书绑定的域名

如果网页地址栏出现绿色小锁标志，表示证书安装成功。



验证证书是否安装成功时，如果网站无法通过https正常访问，需确认您安装证书的服务器443端口是否已开启或被其他工具拦截。