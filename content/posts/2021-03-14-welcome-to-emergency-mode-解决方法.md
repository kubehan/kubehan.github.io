---
title: Welcome to emergency mode! 解决方法
author: Kubehan
type: post
date: 2021-03-14T08:44:14+08:00
url: /3198.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2021/03/1615711329-0fdaa5bbc12eeef.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 1501
categories:
  - Linux运维

---
CentOS 7开机出现  
welcome to emergency mode! 解决方法  
表现为开机出现以下现象  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/03/1615711199-a5ca30d81edd15e.png" alt="file" /> 

大意为：进入紧急mode，输入root密码后再输入journalctl -xb查看日志后再重启系统  
日志如下  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/03/1615711329-0fdaa5bbc12eeef.png" alt="file" />  
排查是因为我之前在/etc/fstab写入挂载分区，但开机有没有挂载成功导致的。

    我的错误原因是挂载磁盘的文件系统格式写错了，导致挂载失败，因此系统启动就到此页面了！

处理办法：自动挂载的那个fstab文件有问题，你  
在这个界面直接输入密码，然后把你增加的删除，重启就OK  
1：登陆root 乱码也输入密码  
2： vim /etc/fstab ，检查磁盘挂载信息  
3：注释掉自己增加的内容，如果确定不在使用可以删除  
4：重启OK。

报这个错误多数情况下是因为/etc/fstab文件的错误。注意一下是不是加载了外部硬盘、存储器或者是网络共享空间，在重启时没有加载上导致的。