---
title: Wordpress 的目录权限应该如何设置？
author: Kubehan
type: post
date: 2022-01-06T02:41:14+08:00
url: /3232.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2022/01/1641436894-cf5303acb7aeac3.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 913
bigger_cover:
  - https://www.kubehan.cn/wp-content/uploads/posterimg/poster-3232.png
categories:
  - Linux运维

---
wordpress 权限对安装和使用效果的影响很大，如果你的服务器使用的是linux系统，可以使用cd命令到你需要修改权限的文件或文件夹所在的目录，然后使用chmod的命令来修改文件权限。  
一、不能安装theme或者修改theme或删除theme

<pre><code class="language-shell">chmod 755 wordpress
find wordpress -type d -exec chmod 755 {} \;
find wordpress -iname “*.php” -exec chmod 644 {} \;
chown -R nginx:nginx wordpress</code></pre>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2022/01/1641436894-cf5303acb7aeac3-300x127.png" alt="" />