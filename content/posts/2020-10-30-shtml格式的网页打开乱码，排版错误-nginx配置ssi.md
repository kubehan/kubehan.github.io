---
title: shtml格式的网页打开乱码，排版错误---Nginx配置ssi
author: Kubehan
type: post
date: 2020-10-30T04:00:35+08:00
url: /3065.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/10/1604029831-a3d10edda3e313b.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 726
categories:
  - Nginx

---
# shtml格式的网页打开乱码，排版错误\---Nginx配置ssi

今天上线一套网站，他的网页形式是shtml格式的，等Nginx Tomcat全部部署完毕后，发现打开网页是乱码的。排版也特别不正常，  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/10/1604029831-a3d10edda3e313b.png" alt="file" />  
网上找了相关资料后得出下文，特此记录

## SSI简介

Server Side Include ： 服务器端嵌入

原理 ： 将内容发送到浏览器之前，可以使用“服务器端包含 (SSI）”指令将文本、图形或应用程序信息包含到网页中。因为包含 SSI 指令的文件要求特殊处理，所以必须为所有 SSI 文件赋予 SSI文件扩展名。默认扩展名是 .stm、.shtm 和 .shtml

主要有以下几种用用途：

  1. 显示服务器端环境变量<#echo>
  2. 将文本内容直接插入到文档中<#include>
  3. 显示WEB文档相关信息<#flastmod #fsize> （如文件制作日期/大小等）
  4. 直接执行服务器上的各种程序<#exec>(如CGI或其他可执行程序）
  5. 设置SSI信息显示格式<#config>；（如文件制作日期/大小显示方式） 高级SSI<XSSI>；可设置变量使用if条件语句。

## 为什么用SSI？

  1. 一个登录用户在页面访问的时候如何充分利用 cache?
  2. 页面静态化的一个大问题是登录用户访问页面如何静态化。例如首页，大部分的页面内容需要缓存但是用户登录后的个人信息是动态信息，不能缓存。那么如何解决这个"页面部分缓存"问题？
  3. 现有的方案是利用 SSI - Server Side include 。
  4. 这里最关键的就是静态文件可以包含一个动态的网页的 URL 。

## Nginx 如何开启 SSI

首先找到你的nginx配置文件

<pre><code class="language-bash">vi /usr/local/nginx/conf/nginx.conf</code></pre>

加入如下代码  
1、开启shtml后缀的文件名支持ssi

<pre><code class="language-bash">server{
    ......
    ssi on;
    ssi_silent_errors on;
    ssi_types text/shtml;
}</code></pre>

2、开启html后缀的文件名支持ssi

<pre><code class="language-bash">server{
    ......
    ssi on;
    ssi_silent_errors on;
}</code></pre>

3、在sample目录下开启html后缀的文件名支持ssi

<pre><code class="language-bash">server{
    ......
    location /sample/{
        ssi on;
        ssi_silent_errors on;
    }
}</code></pre>

保存 重启 nginx