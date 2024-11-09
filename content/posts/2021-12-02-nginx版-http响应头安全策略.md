---
title: Nginx版-HTTP响应头安全策略
author: Kubehan
type: post
date: 2021-12-02T01:24:13+08:00
url: /3215.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 1277
categories:
  - Nginx

---
# HTTP响应头安全策略

## 全部配置如下（可能有遗漏）

<pre><code class="language-bash">add_header Content-Security-Policy "default-src &#039;self&#039; xxx.xxx.com(允许的地址)
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1; mode=block";
add_header X-Frame-Options SAMEORIGIN;
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
add_header &#039;Referrer-Policy&#039; &#039;origin&#039;; 
add_header X-Download-Options noopen;
add_header X-Permitted-Cross-Domain-Policies none;</code></pre>

### Content-Security-Policy

应对漏洞：XSS攻击

<pre><code class="language-bash">控制平台只能执行同源的或者规定源（网址）的js、css等脚本。该配置由浏览器执行，启动后，不符合规则的外部资源就会被拒绝，从而一定程度屏蔽了XSS攻击。</code></pre>

配置策略：

<pre><code class="language-bash">script-src：外部脚本 定义针对 JavaScript 的加载策略
style-src：样式表  定义针对样式的加载策略
img-src：图像   定义针对图片的加载策略
media-src：媒体文件（音频和视频）  定义针对多媒体的加载策略
font-src：字体文件   定义针对字体的加载策略。
object-src：插件（比如 Flash） 定义针对插件的加载策略
child-src：框架    定义针对框架的加载策略
frame-ancestors：嵌入的外部资源（比如&lt;frame&gt;、&lt;iframe&gt;、&lt;embed&gt;和&lt;applet&gt;）
connect-src：HTTP 连接（通过 XHR、WebSockets、EventSource等）定义针对 Ajax/WebSocket 等请求的加载策略。不允许的情况下，浏览器会模拟一个状态为400的响应
worker-src：worker脚本
manifest-src：manifest 文件
##################################################
sandbox : 定义针对 sandbox 的限制
report-uri : 告诉浏览器如果请求的资源不被策略允许时，往哪个地址提交日志信息
form-action : 定义针对提交的 form 到特定来源的加载策略。
referrer : 定义针对 referrer 的加载策略
reflected-xss : 定义针对 XSS 过滤器使用策略。

add_header "Content-Security-Policy" "default-src &#039;self&#039; *.zhixueyun.com &#039;unsafe-inline&#039; &#039;unsafe-eval&#039;; script-src &#039;self&#039; *.zhixueyun.com; frame-ancestors &#039;self&#039; *.zhixueyun.com; object-src &#039;none&#039; *.zhixueyun.com";

add_header "Content-Security-Policy" "default-src  &#039;self&#039; ehr.abc  *.abchina.com  &#039;unsafe-inline&#039;  &#039;unsafe-eval&#039; blob: data: ;"

add_header "Content-Security-Policy" "default-src &#039;self&#039; *.zhixueyun.com &#039;unsafe-inline&#039; &#039;unsafe-eval&#039;; script-src &#039;self&#039; *.zhixueyun.com; frame-ancestors &#039;self&#039; *.zhixueyun.com; object-src &#039;none&#039; *.zhixueyun.com";</code></pre>

每个限制选项可以设置以下几种值，这些值就构成了白名单

<pre><code class="language-bash">主机名：example.org，https://example.com:443
路径名：example.org/resources/js/
通配符：*.example.org，*://*.example.com:*（表示任意协议、任意子域名、任意端口）
协议名：https:、data:
关键字&#039;self&#039;：当前域名，需要加引号
关键字&#039;none&#039;：禁止加载任何外部资源，需要加引号

#指令值    说明
  *               允许加载任何内容
‘none‘            允许加载任何内容
‘self‘            允许加载相同源的内容
www.a.com         允许加载指定域名的资源
*.a.com           允许加载 a.com 任何子域名的资源
https://a.com     允许加载 a.com 的 https 资源
https:            允许加载 https 资源
data:             允许加载 data: 协议，例如：base64编码的图片
‘unsafe-inline‘   允许加载 inline 资源，例如style属性、onclick、inline js、inline css等
‘unsafe-eval‘     允许加载动态 js 代码，例如 eval()</code></pre>

### X-Content-Type-Options

约定资源的响应头，屏蔽内容嗅探攻击。

应对漏洞：内容嗅探攻击

<pre><code class="language-bash">网络请求中，每个资源都有自己的类型，比如Content-Type:text/html 、image/png、 text/css。但是有一些资源的类型是未定义或者定义错了，导致浏览器会猜测资源类型，尝试解析内容，从而给了脚本攻击可乘之机。比如利用一个图片资源去执行一个恶意脚本。

通过设置 X-Content-Type-Options: nosniff ，浏览器严格匹配资源类型，会拒绝加载错误或者不匹配的资源类型。（PS：排查问题过程中，网上看到一个人用流的方式从后台给前台传图片，大概代码&lt;img src="xxxxx.xxxx.do"&gt; 由于后端未指定资源类型，导致增加该配置后无法显示图片）</code></pre>

### X-XSS-Protection

开启浏览器XSS防护(原理不明，待研究.好像是浏览器自己有个filter，能过滤xss攻击脚本)。开启后不会影响业务，无特殊情况，建议开启。

应对漏洞：XSS攻击

配置参数：

    X-XSS-Protection: 0 关闭防护
    X-XSS-Protection: 1 开启防护
    X-XSS-Protection: 1; mode=block 开启防护 如果被攻击，阻止脚本执行。
    
    nginx配置：add_header X-XSS-Protection "1; mode=block";

### X-Frame-Options

开个此属性，控制页面是否可以用于ifream中，如果使用，是什么范围。也可以通过这个控制来避免自己的资源页面被其他页面引用。

应对漏洞：点击劫持

攻击者会用一个自己网站，用ifream或者fream嵌套的方式引入目标网站，诱使用户点击。从而劫持用户点击事件。

配置的三个参数：

    deny 标识该页面不允许在frame中展示，即便在相同域名的页面中嵌套也不行。  
    sameorigin 可以在同域名的页面中frame中展示
    allow-form url 指定的fream中展示。
    nginx配置:add_header X-Frame-Options SAMEORIGIN;

### Strict-Transport-Security

告诉浏览器只能通过https访问当前资源。

    nginx配置：add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";

说明：在接下来的一年（即31536000秒）中，浏览器只要向xxx或其子域名发送HTTP请求时，必须采用HTTPS来发起连接。

### Referrer-Policy

用来规定什么情况下显示referer字段，以及referer字段显示的内容多少。

漏洞说明：

当用户在浏览器上点击一个链接时，会产生一个 HTTP 请求，用于获取新的页面内容，而在该请求的报头中，会包含一个 Referrer，用以指定该请求是从哪个页面跳转页来的，常被用于分析用户来源等信息。但是也成为了一个不安全的因素，所以就有了 Referrer-Policy，用于过滤 Referrer 报头内容

<pre><code class="language-bash">配置参数：no-referrer-when-downgrade：在同等安全等级下（例如https页面请求https地址），发送referer，但当请求方低于发送方（例如https页面请求http地址），不发送refererorigin：仅仅发送origin，即protocal+hostorigin-when-cross-origin：跨域时发送originsame-origin：当双方origin相同时发送strict-origin：当双方origin相同且安全等级相同时发送unfafe-url：任何情况下都显示完整的referernginx配置：add_header &#039;Referrer-Policy&#039; &#039;origin&#039;; </code></pre>

### X-Permitted-Cross-Domain-Policies

<pre><code class="language-bash">一个针对flash的安全策略，用于指定当不能将"crossdomain.xml"文件（当需要从别的域名中的某个文件中读取 Flash 内容时用于进行必要设置的策略文件）放置在网站根目录等场合时采取的替代策略。配置参数：X-Permitted-Cross-Domain-Policies: master-onlymaster-only 只允许使用主策略文件（/crossdomain.xml）add_header X-Permitted-Cross-Domain-Policies none;</code></pre>

### X-Download-Options

<pre><code class="language-bash">用于控制浏览器下载文件是否支持直接打开，如果支持直接打开，可能会有安全隐患。配置信息：X-Download-Options: noopennoopen 用于指定IE 8以上版本的用户不打开文件而直接保存文件。在下载对话框中不显示“打开”选项。nginx配置：add_header X-Download-Options noopen;</code></pre>