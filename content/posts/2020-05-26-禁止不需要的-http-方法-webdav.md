---
title: 禁止不需要的 HTTP 方法--WebDAV
author: Kubehan
type: post
date: 2020-05-26T09:18:03+08:00
url: /2602.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/05/公众号.jpg
Baidusubmit:
  - 1
views:
  - 1055
zm_like:
  - 3
baidu_record:
  - 1
categories:
  - Linux运维

---
WebDAV （Web-based Distributed Authoring and Versioning）是基于 HTTP 1.1 的一个通信协议。它为 HTTP 1.1 添加了一些扩展（就是在 GET、POST、HEAD 等几个 HTTP 标准方法以外添加了一些新的方法），使得应用程序可以直接将文件写到 Web Server 上，并且在写文件时候可以对文件加锁，写完后对文件解锁，还可以支持对文件所做的版本控制。这个协议的出现极大地增加了 Web 作为一种创作媒体对于我们的价值。基于 WebDAV 可以实现一个功能强大的内容管理系统或者配置管理系统。  
现在主流的WEB服务器一般都支持WebDAV，使用WebDAV的方便性，呵呵，就不用多说了吧，用过VS.NET开发ASP.NET应用的朋友就应该 知道，新建/修改WEB项目，其实就是通过WebDAV+FrontPage扩展做到的，下面我就较详细的介绍一下，WebDAV在tomcat中的配 置。  
如何禁止  
如何禁止DELETE、PUT、OPTIONS、TRACE、HEAD等协议访问应用程序应用程序呢？  
解决方法  
第一步：修改应用程序的web.xml文件的协议

<pre><code class="language-xml">&lt;?xml version="1.0" encoding="UTF-8"?&gt;  
&lt;web-app xmlns="http://java.sun.com/xml/ns/j2ee"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
   xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"  
    version="2.4"&gt; </code></pre>

第二步：在应用程序的web.xml中添加如下的代码即可

<pre><code class="language-xml">&lt;security-constraint&gt;  
   &lt;web-resource-collection&gt;  
      &lt;url-pattern&gt;/*&lt;/url-pattern&gt;  
      &lt;http-method&gt;PUT&lt;/http-method&gt;  
&lt;http-method&gt;DELETE&lt;/http-method&gt;  
&lt;http-method&gt;HEAD&lt;/http-method&gt;  
&lt;http-method&gt;OPTIONS&lt;/http-method&gt;  
&lt;http-method&gt;TRACE&lt;/http-method&gt;  
   &lt;/web-resource-collection&gt;  
   &lt;auth-constraint&gt;  
   &lt;/auth-constraint&gt;  
 &lt;/security-constraint&gt;  
 &lt;login-config&gt;  
   &lt;auth-method&gt;BASIC&lt;/auth-method&gt;  
 &lt;/login-config&gt; </code></pre>