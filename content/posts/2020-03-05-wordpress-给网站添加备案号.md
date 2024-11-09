---
title: WordPress 给网站添加备案号
author: Kubehan
type: post
date: 2020-03-05T08:29:48+08:00
url: /623.html
views:
  - 904
  - 904
dwqr_like:
  - 1
  - 1
post_views_count:
  - 1
  - 1
zm_favorites:
  - 1
  - 1
zm_like:
  - 1
  - 1
categories:
  - Linux运维
tags:
  - Wordpress

---
# WordPress 给网站添加备案号

这几天在网上搜了好多添加备案信息的，一堆不靠谱的，这里记录一下我的实际成果

找到WordPress后台–>外观–>主题编辑器–>footer.php 添加

备案号：蜀ICP备<a href="http://www.beian.miit.gov.cn" rel="nofollow">122号-1</a>  
1  
在哪个位置添加呢？？？？  
访问你的网站然后F12查看 代码找到节点，然后在后台 ctrl+F5 在footer.php里面搜索对应的节点 添加到后面即可  
这里我的是

复制上面的添加部分 把这里的信息变更为你的刷新后就行了  
—————————————  
原文链接：https://blog.csdn.net/qq_42568611/article/details/104677536