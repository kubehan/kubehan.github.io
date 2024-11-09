---
title: Tomcat配置https证书
author: Kubehan
type: post
date: 2020-03-13T09:18:06+08:00
url: /983.html
views:
  - 752
  - 752
zm_like:
  - 2
  - 2
post_views_count:
  - 0
  - 0
categories:
  - Tomcat
  - 优化之路
tags:
  - tomcat

---
<span style="background-color: rgb(255, 0, 0);">配置tomcat的https访问时，将原来的http注释掉，然后换成下面的代码就行了</span>

<pre class="brush:bash;toolbar:false;">&lt;Connector&nbsp;port="443"
&nbsp;&nbsp;&nbsp;&nbsp;protocol="HTTP/1.1"
&nbsp;&nbsp;&nbsp;&nbsp;SSLEnabled="true"
&nbsp;&nbsp;&nbsp;&nbsp;scheme="https"
&nbsp;&nbsp;&nbsp;&nbsp;secure="true"
&nbsp;&nbsp;&nbsp;&nbsp;keystoreFile="/usr/local/tomcat_jqt/cert/jqt.oppo.com.cn.pfx"&nbsp;#证书名称前需加上证书的绝对路径
&nbsp;&nbsp;&nbsp;&nbsp;keystoreType="PKCS12"
&nbsp;&nbsp;&nbsp;&nbsp;keystorePass="lpmduVEW"&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;clientAuth="false"
&nbsp;&nbsp;&nbsp;&nbsp;SSLProtocol="TLSv1+TLSv1.1+TLSv1.2"
&nbsp;&nbsp;&nbsp;&nbsp;ciphers="TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA256"/&gt;</pre>