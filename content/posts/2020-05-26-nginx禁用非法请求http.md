---
title: Nginx禁用非法请求http
author: Kubehan
type: post
date: 2020-05-26T09:15:32+08:00
url: /2601.html
Baidusubmit:
  - 1
views:
  - 1352
zm_like:
  - 2
categories:
  - Linux运维

---
全局配置一下代码就OK了

将一下代码添加到nginx的server模块或者location模块

<pre><code class="language-php">if ($request_method ~ ^(PUT|DELETE)$) {
     return 403;
}
</code></pre>

或者

<pre><code class="language-bash">if ($request_method !~ ^(GET|POST)$) {
 return 403;
 }</code></pre>