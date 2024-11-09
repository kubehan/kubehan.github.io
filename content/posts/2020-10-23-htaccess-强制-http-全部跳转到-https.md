---
title: .htaccess 强制 HTTP 全部跳转到 HTTPS
author: Kubehan
type: post
date: 2020-10-23T03:32:03+08:00
url: /3033.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 716
categories:
  - Linux性能优化
  - Linux运维
  - 小技巧

---
HTTP 80 强制转 HTTPS

全站采用https协议访问，所以需要http重定向到https，只需要在.htaccess加入下面规则

在相应的网站根目录新建 .htaccess

<pre><code class="language-shell">RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R,L]</code></pre>

或者

<pre><code class="language-shell"> RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^(.*) https://%{SERVER_NAME}/$1 [R,L]</code></pre>

这里分享几个实战案例  
1、强制301重定向 HTTPS

<pre><code class="language-shell">&lt;IfModule mod_rewrite.c&gt;
RewriteEngine on
RewriteBase /
RewriteCond %{SERVER_PORT} !^443$
RewriteRule (.*) https://%{SERVER_NAME}/$1 [R=301,L]
&lt;/IfModule</code></pre>

2、站点绑定多个域名  
只允许www.kubehan.cn 跳转

<pre><code class="language-shell">RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteCond %{HTTP_HOST} ^pythondesign.cn [NC,OR]
RewriteCond %{HTTP_HOST} ^www.kubehan.cn [NC]
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R,L]</code></pre>

3、一些比较高级的用法，仅供参考

<pre><code class="language-shell">RewriteEngine on
# 强制HTTPS
RewriteCond %{HTTPS} !=on [OR]
RewriteCond %{SERVER_PORT} 80
# 某些页面强制
RewriteCond %{REQUEST_URI} ^something_secure [OR]
RewriteCond %{REQUEST_URI} ^something_else_secure
RewriteRule .* https://%{SERVER_NAME}%{REQUEST_URI} [R=301,L]
# 强制HTTP
RewriteCond %{HTTPS} =on [OR]
RewriteCond %{SERVER_PORT} 443
# 某些页面强制
RewriteCond %{REQUEST_URI} ^something_public [OR]
RewriteCond %{REQUEST_URI} ^something_else_public
RewriteRule .* http://%{SERVER_NAME}%{REQUEST_URI} [R=301,L]</code></pre>

4、只要求访问http://bo.kevin.com/beijing/ 时强制跳转到https://bo.kevin.com/beijing/,其他的url访问时都不做http到https的强转!

下面的配置,就实现了只是针对http://bo.kevin.com/beijing/这一个单独的url做https的强制跳转,其他url访问时都不做跳转!

<pre><code class="language-shell">&lt;IfModule mod_rewrite.c&gt;
RewriteEngine on
RewriteBase /
RewriteCond %{SERVER_PORT} 80
RewriteCond %{HTTP_HOST} ^bo.kevin.com/beijing/ [NC]
RewriteRule ^(.*)$ https://bo.kevin.com/beijing/ [R,L]
&lt;/IfModule&gt;</code></pre>