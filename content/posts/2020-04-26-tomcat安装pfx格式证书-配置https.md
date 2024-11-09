---
title: Tomcat安装PFX格式证书--配置https
author: Kubehan
type: post
date: 2020-04-26T08:27:36+08:00
url: /2309.html
Baidusubmit:
  - 1
views:
  - 932
zm_like:
  - 3
categories:
  - Linux运维

---
前提条件：

  1. 您的Tomcat服务器上已经开启了443端口（HTTPS服务的默认端口）。
  2. 已安装OpenSSL工具。
  3. 已下载Tomcat服务器所需要的证书文件
  4. 已登录您的Tomcat服务器。  
    说明：  
    Tomcat 9强制要求证书别名设置为tomcat。您需要使用以下keytool命令将protocol="HTTP/1.1"转换成protocol="org.apache.coyote.http11.Http11NioProtocol"</p> 
    <pre><code class="language-bash">keytool -changealias -keystore domain name.pfx -alias alias -destalias tomcat</code></pre>

证书文件名称为domain name.pfx，  
证书密码文件名称为pfx-password.txt。  
操作步骤：  
1.解压已下载保存到本地的Tomcat证书文件并且上传到服务器  
2.在Tomcat安装目录下新建cert目录，将解压的证书和密码文件拷贝到cert目录下。  
3.修改配置文件server.xml，并保存。  
文件路径：Tomcat安装目录/conf/server.xml  
去掉以下内容的注释

<pre><code class="language-bash">&lt;Connector  port="8443"
protocol="HTTP/1.1"
port="8443" SSLEnabled="true"
maxThreads="150" scheme="https" secure="true"
clientAuth="false" sslProtocol="TLS" /&gt;</code></pre>

参照以下内容修改<Connector port="443"标签内容

<pre><code class="language-bash">&lt;Connector port="443"   #port属性根据实际情况修改（https默认端口为443）。如果使用其他端口号，则您需要使用https://yourdomain:port的方式来访问您的网站。
protocol="HTTP/1.1"
SSLEnabled="true"
scheme="https"
secure="true"
keystoreFile="Tomcat安装目录/cert/domain name.pfx" #证书名称前需加上证书的绝对路径，请使用您证书的文件名替换domain name。
keystoreType="PKCS12"
keystorePass="证书密码"  #请替换为密码文件pfx-password.txt中的内容。
clientAuth="false"
SSLProtocol="TLSv1+TLSv1.1+TLSv1.2"
ciphers="TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA256"/&gt;</code></pre>

4.配置web.xml文件，开启HTTP强制跳转HTTPS  
在文件</welcome-file-list>后添加以下内容：

<pre><code class="language-bash">&lt;login-config&gt;  
&lt;!-- Authorization setting for SSL --&gt;  
&lt;auth-method&gt;CLIENT-CERT&lt;/auth-method&gt;  
&lt;realm-name&gt;Client Cert Users-only Area&lt;/realm-name&gt;  
&lt;/login-config&gt;  
&lt;security-constraint&gt;  
&lt;!-- Authorization setting for SSL --&gt;  
&lt;web-resource-collection &gt;  
    &lt;web-resource-name &gt;SSL&lt;/web-resource-name&gt;  
    &lt;url-pattern&gt;/*&lt;/url-pattern&gt;  
&lt;/web-resource-collection&gt;  
&lt;user-data-constraint&gt;  
    &lt;transport-guarantee&gt;CONFIDENTIAL&lt;/transport-guarantee&gt;  
&lt;/user-data-constraint&gt;  
&lt;/security-constraint&gt;</code></pre>

5.重启Tomcat。  
访问https://您的域名  
如果网页地址栏出现绿色小锁标志，表示证书安装成功。  
验证证书是否安装成功时，如果网站无法通过https正常访问，需确认您安装证书的服务器443端口是否已开启或被其他工具拦截。