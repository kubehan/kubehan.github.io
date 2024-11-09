---
title: Apache目录列表漏洞修复方法---icons
author: Kubehan
type: post
date: 2020-08-19T07:46:19+08:00
url: /2754.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/08/1597822934-6ba41503e3989e6.png
views:
  - 1255
post_style:
  - sidebar
cao_vip_rate:
  - 1
categories:
  - Linux性能优化
  - Linux运维
  - 优化之路
  - 基线检查
  - 漏洞公告

---
接到市总的通知。网站有目录列表显示的漏洞，用的apache服务器  
漏洞描述：使用Apache服务器时，访问地址：<https://您的域名/icons/>  
会显示你的网站的icon信息

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/08/1597822934-6ba41503e3989e6.png" alt="file" /> 

关闭apache自动目录列表功能的方法 按如下步骤可以禁止：  
打开目录apache/conf/extra/下的文件httpd-autoindex.conf（位置可能有差异）  
我的apache是yum安装的  
配置文件路径：/etc/httpd/conf.d/autoindex.conf  
找到

<pre><code class="language-bash">Alias /icons/ "/usr/share/httpd/icons/"
&lt;Directory "/usr/share/httpd/icons"&gt;
    Options Indexes MultiViews FollowSymlinks
    AllowOverride None
    Require all granted
&lt;/Directory&gt;</code></pre>

去掉Indexes改成

<pre><code class="language-bash">Alias /icons/ "/usr/share/httpd/icons/"
&lt;Directory "/usr/share/httpd/icons"&gt;
    Options  MultiViews FollowSymlinks
    AllowOverride None
    Require all granted
&lt;/Directory&gt;</code></pre>

重启apache服务器！

暴力解决办法就是注释掉或者直接删除icons目录