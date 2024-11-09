---
title: Woprdpress 底部友情链接+自助申请友情链接模块
author: Kubehan
type: post
date: 2020-10-21T11:45:19+08:00
url: /3026.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/10/1603274244-fa7d7b4ba9fdfee.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 1467
categories:
  - Linux运维

---
此教程与网上的教程有一定的差别，实现的效果基本差不多，新增了一条网站描述表单

教程开始：

  1. 将blinks.php 上传到主题目录下的/pages文件夹
  2. 将diy.css 内容复制到主题目录下的css文件夹下的css样式文件diy.css

<pre><code class="language-php">    ripro主题路径为：/assets/css/diy.css
    其他主题添加到:  style.css</code></pre>

<ol start="3">
  <li>
    将diy-footer.php 文件内容复制到：
  </li>
</ol>

<pre><code class="language-php">主题目录下的footer.php文件最后&lt;/div&gt;前面，需要让移动端显示的话就放到&lt;/div&gt;后面
    ripro主题路径为： /parts/diy-footer.php
    其他主题添加到: footer.php</code></pre>

<ol start="4">
  <li>
    登录后台添加页面:名称及内容均填写apply-links
  </li>
</ol>

效果展示：  
一、效果1  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/10/1603274212-8036bfd704ad585.png" alt="" /> 

一、效果2  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/10/1603274244-fa7d7b4ba9fdfee.png" alt="" />