---
title: Shell语句之case判断模板
author: Kubehan
type: post
date: 2020-03-03T09:54:57+08:00
url: /456.html
wb_dl_type:
  - 1
  - 1
views:
  - 799
  - 799
wb_dl_mode:
  - 0
  - 0
wb_down_local_url:
  - 
  - 
wb_down_url_ct:
  - 
  - 
wb_down_url:
  - 
  - 
wb_down_pwd:
  - 
  - 
post_views_count:
  - 0
  - 0
zm_like:
  - 1
categories:
  - Linux运维
tags:
  - Shell

---
<!-- wp:code -->

<pre class="wp-block-code"><code>read -p "提示符&#91;y/n]：" i
case $i in
      &#91;Y,y])
	shell命令块
;;
      &#91;N,n])
	shell命令块
;;
       *)
            echo "请输入Y/y or  N/n"
esac</code></pre>

<!-- /wp:code -->