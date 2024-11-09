---
title: PHPCMS全站支持https教程
author: Kubehan
type: post
date: 2020-09-22T09:13:31+08:00
url: /2900.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/05/1600761460-99fdc41c82dc1ed.png
post_style:
  - sidebar
cao_price:
  - 10
cao_vip_rate:
  - 1
cao_diy_btn:
  - 打赏
cao_paynum:
  - 213
views:
  - 1410
categories:
  - Linux运维

---
程序修改部分

1、修改phpcms/modules/admin/site.php  
大约45行和128行的正则

<pre><code class="language-php">(&#039;/http:\/\/(.+)\/$/i&#039;, $domain))</code></pre>

修改为：

<pre><code class="language-php">(&#039;/(http|https):\/\/(.+)\/$/i&#039;, $domain))</code></pre>

2、修改phpcms/modules/admin/templates/setting.tpl.php大约18行中的正则

<pre><code class="language-php">http:\/\/(.+)[^/]$</code></pre>

修改为：

<pre><code class="language-php">http[s]?:\/\/(.+)[^/]$</code></pre>

3、修改phpcms/modules/admin/templates/site_add.tpl.php  
大约13行中的正则

<pre><code class="language-php">http:\/\/(.+)\/$</code></pre>

修改为：

<pre><code class="language-php">http[s]?:\/\/(.+)\/$</code></pre>

4、修改phpcms/modules/admin/templates/site_edit.tpl.php大约11行中的正则

<pre><code class="language-php">http:\/\/(.+)\/$</code></pre>

修改为：

<pre><code class="language-php">http[s]?:\/\/(.+)\/$</code></pre>

5、修改phpcms/modules/link/templates/link_add.tpl.php大约10行中的正则

<pre><code class="language-php">^http:\/\/[A-Za-z0-9]+\.[A-Za-z0-9]+[\/=\?%\-&]*([^&lt;&gt;])*$</code></pre>

修改为：

<pre><code class="language-php">^http[s]?:\/\/[A-Za-z0-9]+\.[A-Za-z0-9]+[\/=\?%\-&]*([^&lt;&gt;])*$</code></pre>

6、修改phpcms/modules/link/templates/link_edit.tpl.php大约11行中的正则

<pre><code class="language-php">^http:\/\/[A-Za-z0-9]+\.[A-Za-z0-9]+[\/=\?%\-&]*([^&lt;&gt;])*$</code></pre>

修改为：

<pre><code class="language-php">^http[s]?:\/\/[A-Za-z0-9]+\.[A-Za-z0-9]+[\/=\?%\-&]*([^&lt;&gt;])*$</code></pre>

7、修改phpcms/modules/link/index.php大约41行和51行中的正则

<pre><code class="language-php">/http:\/\/(.*)/i</code></pre>

修改为：

<pre><code class="language-php">/^http[s]?:\/\/(.*)/i</code></pre>

8、修改phpcms/libs/functions/global.func.php  
大约738行的正则

<pre><code class="language-php">$url = str_replace(array(&#039;http://&#039;,&#039;//&#039;,&#039;~&#039;), array(&#039;~&#039;,&#039;/&#039;,&#039;http://&#039;), $url);</code></pre>

修改为

<pre><code class="language-php">$url = str_replace(array(&#039;https://&#039;,&#039;//&#039;,&#039;~&#039;), array(&#039;~&#039;,&#039;/&#039;,&#039;https://&#039;), $url);</code></pre>

9、修改phpcms/modules/content/templates/content_list.tpl.php

大约97行

<pre><code class="language-php">elseif(strpos($r[&#039;url&#039;],&#039;http://&#039;)!==false)</code></pre>

修改为

<pre><code class="language-php">elseif(strpos($r[&#039;url&#039;],&#039;https://&#039;)!==false)</code></pre>

10、修改phpcms/modules/content/templates/content_page.tpl.php

大约28行

<pre><code class="language-php">if(strpos($category[&#039;url&#039;],&#039;http://&#039;)===false)</code></pre>

修改为

<pre><code class="language-php">if(strpos($category[&#039;url&#039;],&#039;http://&#039;)===false && strpos($category[&#039;url&#039;],&#039;https://&#039;)===false)</code></pre>

接着修改后台设置->基本设置 对应的css、js、图片、附件的路径全改为https

还有后台设置-> 站点管理 里面的站点域名http改为https

如果你的网站是使用了https  但是url没有显示绿色的安全 则需要在head头部代码 加入

<pre><code class="language-php">&lt;meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests" /&gt;</code></pre>

最后更新站点首页，内容页，栏目页。

严格按照以上步骤修改后，注册用户、帐号登录等操作完全正常，和PHPSSO通信完全正常，后台添加信息和前台链接URL完全正常

[phpcms添加ssl证书实现https访问后后台列表页不能翻页的解决][1]

 [1]: https://www.kubehan.cn/3034.html "phpcms添加ssl证书实现https访问后后台列表页不能翻页的解决"