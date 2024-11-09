---
title: phpcms添加ssl证书实现https访问后后台列表页不能翻页的解决
author: Kubehan
type: post
date: 2020-10-23T03:39:35+08:00
url: /3034.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/10/1603424124-b357678a786f959.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 768
bigger_cover:
  - https://www.kubehan.cn/wp-content/uploads/posterimg/poster-3034.png
categories:
  - Linux运维

---
# phpcms添加ssl证书实现https访问后后台列表页不能翻页的解决

前不久时间做等保二级，要求万或者使用https访问，用的phpcms，把ssl搞定后发现内页管理内内容无法翻页，点击无反应，  
让phpcms全站支持https访问的教程：  
[PHPCMS全站支持https教程][1]

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/10/1603424124-b357678a786f959.png" alt="file" />  
打开/phpcms/libs/functions/global.func.php,搜索

<pre><code class="language-php">$url = str_replace(array(&#039;http://&#039;,&#039;//&#039;,&#039;~&#039;), array(&#039;~&#039;,&#039;/&#039;,&#039;http://&#039;), $url);</code></pre>

把上面这个替换成下面的

<pre><code class="language-php">$url = str_replace(array(&#039;https://&#039;,&#039;http://&#039;,&#039;//&#039;,&#039;~&#039;,&#039;~&#039;), array(&#039;~&#039;,&#039;~&#039;,&#039;/&#039;,&#039;https://&#039;,&#039;http://&#039;), $url);</code></pre>

现在去清空缓存，再点击看看就能翻页了

 [1]: https://www.kubehan.cn/2900.html "PHPCMS全站支持https教程"