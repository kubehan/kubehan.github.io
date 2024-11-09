---
title: 记录一下Windows server 2008安装mysql8.0遇到的坑
author: Kubehan
type: post
date: 2020-05-14T07:10:28+08:00
url: /2588.html
Baidusubmit:
  - 1
views:
  - 1176
cms_top:
  - 'true'
cat_top:
  - 'true'
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/05/mysql.png
zm_like:
  - 6
categories:
  - Linux运维
  - Mysql

---
记录一下Windows server 2008安装mysql8.0遇到的坑  
安装教程：[Windows server安装mysql8.0][1]  
背景：  
今天跑到某内网环境部署项目，要求安装mysql数据库，来之前我在官网下载了mysql.8.0.13的zip数据包来安装部署，遇到了下面几个问题，记录一下，方便日后好找  
问题1：数据库初始化错误

<pre><code class="language-shell">2018-10-13T03:29:24.179826Z 0 [System] [MY-010116] [Server] D:\Program Files\MySQL\bin\mysqld.exe (mysqld 8.0.13) starting as process 7420
2018-10-13T03:29:24.205939Z 1 [ERROR] [MY-011011] [Server] Failed to find valid data directory.
2018-10-13T03:29:24.207560Z 0 [ERROR] [MY-010020] [Server] Data Dictionary initialization failed.
2018-10-13T03:29:24.209780Z 0 [ERROR] [MY-010119] [Server] Aborting
2018-10-13T03:29:24.213334Z 0 [System] [MY-010910] [Server] D:\Program Files\MySQL\bin\mysqld.exe: Shutdown complete (mysqld 8.0.13)  MySQL Community Server - GPL.</code></pre>

问题原因：网上查找的答案基本上都是让注释掉datadir这个项目，我感觉是不现实的，刚开始怀疑是my.ini的编写有问题，结果并不是，最终的问题还是文件编码的问题，索性用Notepad++来编写吧，最后就这样成功了

问题1：数据库启动错误

<pre><code class="language-bash">error: Found option without preceding group in config file: D:\mysql-8.0.13-win64\my.ini at line: 1
Fatal error in defaults handling. Program aborted</code></pre>

问题原因： 当时创建my.ini文件时是新建txt文档后改名的，这样的my.ini文件格式可能是utf-8 

解决办法： my.ini文件保存为ANSI格式文件 

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/05/image-1589440120769.png" alt="file" /> 

特别建议方法：先安装一个NotePad++来编写配置文件吧，不然遇到坑还不知道咋回事  
安装mysql8.0注意事项：  
看网上很多安装mysql8.0的教程都要求新建my.ini文件，其实不需要的  
1，mysql8 之后并不需要my.ini，会自动的生成data文件夹在解压之后的文件，端口默认3306,。若有这个文件，则初始化mysql不成功。  
2，自己若新建并设置了my.ini 文件，有data文件的话，在初始化之前要删除。然后再初始化  
3，在初始化之后会自动生成密码，要记下来，后续登上mysql需改密码之后才可后续操作。  
4，要更改加密规则，不然无法使用工具连接mysql

 [1]: https://www.kubehan.cn/2586.html "Windows server安装mysql8.0"