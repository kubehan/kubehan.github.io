---
title: 在服务器上使用docker实现LNMP实验环境
author: Kubehan
type: post
date: 2020-03-24T12:31:58+08:00
url: /1559.html
Baidusubmit:
  - 1
  - 1
views:
  - 1198
  - 1198
zm_like:
  - 2
  - 2
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker7.png
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker7.png
categories:
  - Linux运维

---
<span style="color: black; font-size: 16pt;"><strong>项目主题：在服务器上使用docker实现LNMP实验环境</strong></span>  
<span style="color: black;"><strong><br /> </strong></span>

<span style="font-size: 12pt;"><strong>项目需求</strong></span><span style="font-size: 9pt;">:</span>公司业务扩展，客户需要使用web访问公司数据库记录信息，现公司决定搭建LNMP平台满足此需求。现阶段客户访问量不是太大,采用一台物理服务器搭建docker实现成本的节约。

## 思路图：

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker1.png" alt="" /> 

<p style="background: white;">
  <span style="font-size: 12pt;"><strong>硬件平台:</strong></span>dell R710服务器,内存:16G,cpu:两颗/四核E5504, 硬盘4*300G
</p>

<p style="background: white;">
  <span style="font-size: 12pt;"><strong>软件平台: </strong></span>centos7的服务器系统,docker版本1.10,
</p>

<p style="background: white;">
  centos6.8的基础docker镜像
</p>

<span style="color: #00b0f0; font-family: Arial; font-size: 15pt;"><strong>Docker</strong></span>的优点：

1. 快速交付应用程序

  * <div style="text-align: justify;">
      <span style="color: #333333;"><span style="font-family: 宋体; font-size: 9pt; background-color: white;">开发者使用一个标准的</span><span style="font-family: Arial;"><span style="font-size: 9pt; background-color: white;"><br /> </span><span style="color: red;"><span style="font-size: 12pt; background-color: white;"><strong>image</strong></span><span style="color: #333333; font-size: 9pt; background-color: white;"><br /> </span></span></span><span style="font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">来构建开发容器，开发完成之后，</span><span style="font-family: Arial; background-color: white;"><br /> </span></span></span>
    </div>
    
    <p style="text-align: justify;">
      <span style="color: #333333; font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">系统管理员就可以。使用这个容器来部署代码。</span><span style="font-family: Arial; background-color: white;"><br /> </span></span>
    </p>

  * <div style="text-align: justify;">
      <span style="color: red;"><span style="font-family: Arial; font-size: 12pt; background-color: white;">docker</span><span style="font-size: 9pt;"><span style="color: #333333; font-family: 宋体; background-color: white;">可</span>以<span style="color: #333333;"><span style="font-family: 宋体; background-color: white;">快速创建容器，快速迭代应用程序，并让整个过程可见，</span><span style="font-family: Arial; background-color: white;"><br /> </span></span></span></span>
    </div>
    
    <p style="text-align: justify;">
      <span style="color: #333333; font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">使团队中的其他成员更容易理解应用程序是如何创建和工作的。</span><span style="font-family: Arial; background-color: white;"><br /> </span></span>
    </p>

  * <div style="text-align: justify;">
      <span style="color: red;"><span style="font-family: Arial; font-size: 12pt; background-color: white;">docker</span><span style="color: #333333; font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">容器很轻！很快！容器的启动时间是次秒级的，节约开发、</span><span style="font-family: Arial; background-color: white;"><br /> </span></span></span>
    </div>
    
    <p style="text-align: justify;">
      <span style="color: #333333; font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">测试、部署的时间。</span><span style="font-family: Arial; background-color: white;"><br /> </span></span>
    </p>

<p style="text-align: justify;">
  <span style="font-size: 10pt;">2. 更容易部署和扩展<br /> </span>
</p>

  * <div style="text-align: justify;">
      <span style="color: red;"><span style="font-family: Arial; font-size: 12pt; background-color: white;">docker</span><span style="color: #333333; font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">容器可以在几乎所有的环境中运行，物理机、虚拟机、公有云、</span><span style="font-family: Arial; background-color: white;"><br /> </span></span></span>
    </div>
    
    <p style="text-align: justify;">
      <span style="color: #333333; font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">私有云、个人电脑、服务器等等。</span><span style="font-family: Arial; background-color: white;"><br /> </span></span>
    </p>

  * <div style="text-align: justify;">
      <span style="color: red;"><span style="font-family: Arial;"><span style="font-size: 12pt; background-color: white;">docker</span><span style="color: #333333; font-size: 9pt; background-color: white;"><br /> </span></span><span style="font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">容器兼容很多平台，这样就可以把一个应用程序从一个平台迁移到另外一个。</span><span style="color: #333333; font-family: Arial; background-color: white;"><br /> </span></span></span>
    </div>

<span style="color: #333333; font-family: Arial; font-size: 9pt; background-color: white;">3. </span>效率更高

  * <div style="text-align: justify;">
      <span style="color: red;"><span style="font-size: 12pt;"><strong>d<span style="font-family: Arial; background-color: white;">ocker</span></strong></span><span style="color: #333333;"><span style="font-family: 宋体; font-size: 9pt; background-color: white;">容器不需要</span><span style="color: red;"><span style="font-family: Arial;"><span style="font-size: 12pt; background-color: white;">hypervisor</span><span style="color: #333333; font-size: 9pt; background-color: white;"><br /> </span></span><span style="font-size: 9pt;"><span style="font-family: 宋体; background-color: white;">，他是内核级的虚拟化</span><span style="color: #333333; font-family: Arial; background-color: white;"><br /> </span></span></span></span></span>
    </div>

<span style="color: #333333; font-family: Arial; font-size: 9pt; background-color: white;">4. </span>快速部署也意味着更简单的管理

  * <div style="text-align: justify;">
      <span style="font-size: 9pt;">通<span style="color: #333333;"><span style="font-family: 宋体; background-color: white;">常只需要小小的改变就可以替代以往巨型和大量的更新工作。</span><span style="font-family: Arial; background-color: white;"><br /> </span></span></span>
    </div>

<span style="color: #00b0f0; font-size: 18pt;"><strong><span style="font-family: Arial; background-color: white;">LNMP</span><span style="font-family: 宋体; background-color: white;">：</span></strong></span>Linux系统下Nginx+MySQL+PHP这种网站服务器架构。

<a href="http://baike.baidu.com/view/1634.htm" target="http://baike.baidu.com/_blank" rel="noopener noreferrer"><span style="color: red; font-family: Arial; background-color: white;"><strong>Linux</strong></span></a>是服务器操作系统；<a href="http://baike.baidu.com/view/926025.htm" target="http://baike.baidu.com/_blank" rel="noopener noreferrer"><span style="color: red; font-family: Arial;"><strong>Nginx</strong></span></a>是一个高性能的HTTP和<a href="http://baike.baidu.com/view/1165595.htm" target="http://baike.baidu.com/_blank" rel="noopener noreferrer">反向代理</a>服务器；<span style="color: red;"><strong>Mysql</strong></span>是一个小型<a href="http://baike.baidu.com/view/1450387.htm" target="http://baike.baidu.com/_blank" rel="noopener noreferrer">关系型数据库管理系统</a>；<a href="http://baike.baidu.com/view/99.htm" target="http://baike.baidu.com/_blank" rel="noopener noreferrer"><span style="color: red; font-family: Arial; background-color: white;"><strong>PHP</strong></span></a>是一款超文本预处理器。

这四种软件均为免费<a href="http://baike.baidu.com/view/444964.htm" target="http://baike.baidu.com/_blank" rel="noopener noreferrer"><span style="color: red; font-family: 宋体; background-color: white;"><strong>开源软件</strong></span></a>，组合到一起，成为一个免费、高效、扩展性强的网站服务系统。

<span style="font-size: 12pt;"><strong>项目实施：</strong></span><span style="font-family: 微软雅黑; font-size: 14pt;"><br /> </span>

<span style="font-family: 微软雅黑;"><span style="font-size: 14pt;">部署安装mysql服务</span><br /> </span>

<span style="font-family: 微软雅黑;"><span style="color: #010101; font-size: 10pt;">使用centos6的基础镜像生成容器：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@localhost Desktop]# docker run -dit -v /docker/:/mysql -v /data/:/data --name mysql docker.io/centos:centos6<span style="color: #010101;"> <span style="color: #333333;"><br /> </span></span></span>
</p>

<span style="font-family: 微软雅黑; font-size: 9pt;">-dit ： 分别是指容器可以在后台运行，前端运行，给容器赋予交互式界面<br /> </span>

<span style="font-family: 微软雅黑; font-size: 9pt;">-v :将本地目挂载到容器内部，可实现容器内部访问到宿主机的目录，并实现了容器内的重要数据存储的安全<br /> </span>

<span style="font-family: 微软雅黑;"><span style="font-size: 9pt;">--name： 给容器起个容器名称。<span style="color: red;">(以下生成容器选项含义一致)</span></span><span style="font-size: 12pt;"><br /> </span></span>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">所需软件：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="font-family: 微软雅黑; font-size: 9pt;"><span style="color: #333333;">bison-3.0.4.tar.gz   cmake-3.5.2.tar.gz   ncurses-5.9.tar.gz</span><br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="font-family: 微软雅黑; font-size: 9pt;"><span style="color: #333333;">boost_1_59_0.tar.gz  mysql-5.7.13.tar.gz</span><br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">安装开发所需依赖的安装包：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="font-family: 微软雅黑; font-size: 9pt;"><span style="color: #333333;">[root@213410cda363 /]#yum -y install gcc-c++  m4  perl</span><br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">安装mysql所需软件包：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# tar zxf bison-3.0.4.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# cd bison-3.0.4<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 bison-3.0.4]# ./configure && make && make install<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# tar zxf boost_1_59_0.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# mv boost_1_59_0 /usr/local/boost<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# tar zxf ncurses-5.9.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# cd ncurses-5.9<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 ncurses-5.9]# ./configure && make && make install<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# tar zxf  cmake-3.5.2.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# cd cmake-3.5.2<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 cmake-3.5.2]# ./bootstrap && gmake && gmake install<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql]# tar zxf mysql-5.7.13.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 mysql-5.7.13]#cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/data -DSYSCONFDIR=/etc -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DEXTRA_CHARSETS=all -DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_BOOST=/usr/local/boost && make && make install<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">创建mysql服务运行用户并给安装目录及数据目录授予权限并初始化mysql数据库：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 /]#useradd -s /bin/false -M mysql<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 /]chown -R mysql:mysql /data<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 /]chown -R mysql:mysql /usr/local/mysql/<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 /]/usr/local/mysql/bin/mysqld  --initialize-insecure  --user=mysql  --basedir=/usr/local/mysql --datadir=/data<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="color: #333333; font-size: 12pt;">优化mysql路径：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 data]# tail -1 /etc/profile<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">export PATH=$PATH:/usr/local/mysql/bin<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 data]# . /etc/profile<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">创建mysql的参数文件到/etc下和mysql的启动脚本到/etc/init.d/下添加系统服务</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 support-files]#cp my-default.cnf /etc/my.cnf<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 support-files]#cp -p mysql.server /etc/init.d/mysqld<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 support-files]#chkconfig --add mysqld<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">修改mysql的主配置文件：/etc/my.cnf</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 support-files]# grep  -v "^#" /etc/my.cnf | grep -v  "^;" | grep -v "^$"<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[mysqld]</span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">datadir = /data<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">port = 3306<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">server_id = 1<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">socket = /usr/local/mysql/mysql.sock<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">log-error = /data/mysqld.err<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">启动mysql的服务并查看端口：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 support-files]# service mysqld start<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">Starting MySQL.. SUCCESS!<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@213410cda363 support-files]# netstat -anpt | grep 3306<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">tcp        0      0 :::3306                     :::*                        LISTEN      -<br /> </span>
</p>

<span style="font-family: 微软雅黑; font-size: 9pt;">创建访问mysql服务所需用户：<br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker2.png" alt="" /> <span style="font-family: 微软雅黑; font-size: 9pt;"><br /> </span>

<span style="font-family: 微软雅黑;"><span style="font-size: 14pt;">部署php服务</span><br /> </span>

<span style="font-family: 微软雅黑;"><span style="color: #010101; font-size: 10pt;">使用centos6的基础镜像生成容器：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">docker run -dit -v /docker/:/php -v /php/usr/loca/php5/etc --link mysql --name php docker.io/centos:centos6<span style="color: #010101;"><br /> </span></span>
</p>

<span style="font-family: 微软雅黑; font-size: 9pt;">--link mysql："mysql"是容器名称，php服务器是需要连接mysql服务器的，容器之间使用link命令实现容器互连<span style="color: red;">(以下生成容器选项含义一致)</span><br /> </span>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">所需软件</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="font-family: 微软雅黑; font-size: 9pt;"><span style="color: #333333;">php-5.3.28.tar.gz   ZendGuardLoader-php-5.3-linux-glibc23-x86_64.tar.gz</span><br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">安装部署php依赖软件包和开发依赖包，并指定mysqlclient库文件</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 /]#yum -y install gd libxml2-devel libjpeg-devel libpng-devel mysql-devel gcc-c++<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 /]#cp /usr/lib64/mysql/libmysqlclient.so.16.0.0 /usr/lib/libmysqlclient.so<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">解压php的tar格式压缩包，并配置安装</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 /]#tar zxf php-5.3.28.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 /]#cd php-5.3.28<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 php-5.3.28]# ./configure --prefix=/usr/local/php5 --with-gd --with-zlib --with-mysql --with-mysqli  --with-mysql-sock --with-config-file-path=/usr/local/php5/ --enable-mbstring --enable-fpm --with-jpeg-dir=/usr/lib && make && make install<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">创建php.ini和php-fpm配置文件</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 php-5.3.28]# cp php.ini-production /usr/local/php5/etc/php.ini<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 etc]# cp php-fpm.conf.default php-fpm<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">为了提高php解析效率，将ZendGuardLoader-php-5.3-linux-glibc23-x86_64.tar.gz解压并配置</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 php]# tar zxf ZendGuardLoader-php-5.3-linux-glibc23-x86_64.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 php]#cd ZendGuardLoader-php-5.3-linux-glibc23-x86_64/php-5.3.x<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 php-5.3.x]# cp ZendGuardLoader.so /usr/local/php5/lib/php/<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">在php主配置文件php.ini添加内容，启用ZendGuardLoader：</span><br /> </span>

<p style="background: #fbfaf8;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker3.png" alt="" align="right" /><span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 php]# tail -2 /usr/local/php5/etc/php.ini<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker4.png" alt="" align="left" /><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker5.png" alt="" align="left" /><span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">zend_extension=/usr/local/php5/lib/php/ZendGuardLoader.so<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker6.png" alt="" align="left" /><span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">zend_loader.enable=1<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">创建监听用户，修改php-fpm配置文件以启动php-fpm进程监听php解析请求：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 etc]# useradd -s /bin/false -M php<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 etc]# grep -v "^;" ./php-fpm  | grep -v "^$"<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker7.png" alt="" /><span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;"><br /> </span>
</p>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker8.png" alt="" /> <span style="font-family: 微软雅黑; font-size: 12pt;"><br /> </span>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">启动php-fpm进程并查看进程监听状态</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 etc]# /usr/local/php5/sbin/php-fpm<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@b4dd46498f63 etc]# netstat -anpt | grep  9000<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">tcp        0      0 0.0.0.0:9000                0.0.0.0:*                   LISTEN      808/php-fpm<br /> </span>
</p>

<span style="font-family: 微软雅黑; font-size: 9pt;"> 创建php连接mysql，及php语言创建的测试文件：<br /> </span>

<span style="font-family: 微软雅黑; font-size: 9pt;">创建存放测试文件目录<span style="color: red;">（此目录要与nginx配置文件中的一致）<br /> </span></span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker9.png" alt="" /> 

创建nginx可成功连接php服务器文件：

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker10.png" alt="" /> 

<span style="font-family: 微软雅黑; font-size: 9pt;">Php链接mysql脚本：<br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker11.png" alt="" /> 

<span style="font-family: 微软雅黑; font-size: 19pt;">部署Nginx<br /> </span>

<span style="font-family: 微软雅黑;"><span style="color: #010101; font-size: 10pt;">使用centos6生成nginx容器：</span><br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker12.png" alt="" /> 

-p: nginx是前端监听客户端访问的服务器，所以需要设置监听端口，容器中端口发布时可以使

用--port简写-p命令使容器端口映射到宿主机端口，实现服务发布需求

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">所需软件： </span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="font-family: 微软雅黑; font-size: 9pt;"><span style="color: #333333;">nginx-1.11.2.tar.gz</span><br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">安装Nginx的依赖包并且创建服务运行用户：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@e56461580138 nginx]# yun -y install pcre-devel zlib-devel openssl-devel gcc-c++<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@e56461580138 nginx]# useradd -M -s /bin/false nginx<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;"> 解压并且安装nginx：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@e56461580138 nginx]# tar zxf nginx-1.11.2.tar.gz<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@e56461580138 nginx]# cd nginx-1.11.2<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@e56461580138 nginx-1.11.2]# ./configure --prefix=/usr/local/nginx --with-http_stub_status_module --user=nginx --group=nginx<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@e56461580138 nginx-1.11.2]# make && make install<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">做路径优化：</span><br /> </span>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@e56461580138 nginx-1.11.2]# ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/<br /> </span>
</p>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">修改nginx的住配置文件,使nginx能解析php网页：</span><br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker13.png" alt="" /> <span style="font-family: 微软雅黑;"><br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker14.png" alt="" /> <span style="font-family: 微软雅黑;"><br /> </span>

<span style="font-family: 微软雅黑;"><span style="font-size: 12pt;">检测nginx配置文件并启动服务</span><br /> </span>

<p style="background: #fbfaf8;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker15.png" alt="" align="left" /><span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@d34486af8b6d /]# nginx -t<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">nginx: the configuration file /usr/local/nginx/conf/nginx.conf <span style="background-color: red;">syntax is ok</span><br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@d34486af8b6d /]# nginx<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">[root@d34486af8b6d /]# netstat -anpt | grep nginx<br /> </span>
</p>

<p style="background: #fbfaf8;">
  <span style="color: #333333; font-family: 微软雅黑; font-size: 9pt;">tcp        0      0 0.0.0.0:80                  0.0.0.0:*                   LISTEN      16/nginx<br /> </span>
</p>

<span style="font-family: 微软雅黑; font-size: 12pt;">Lnmp项目到此就做完，进行测试。验证lnmp平台是否可以正常运行：<br /> </span>

<span style="font-family: 微软雅黑; font-size: 9pt;">访问静态网页：<br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker16.png" alt="" align="left" /><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker17.png" alt="" /> 

测试nginx连接php应用：

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker18.png" alt="" align="left" /><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker19.png" alt="" /> 

<span style="color: red;">*经过前两个的测试返回结果。足以证明搭建的lnmp平台顺利实现了web平台的静动分离的效果。<br /> </span>

验证客户端访问mysql数据库的效果：

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker20.png" alt="" align="left" /><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_1258_docker21.png" alt="" /> 

到此，lnmp项目完成。

<span style="color: #333333;"><span style="font-family: 宋体; background-color: white;">六．<span style="font-size: 12pt;"><strong>项目总结</strong></span></span><span style="font-family: Arial; background-color: white;"><strong><br /> </strong></span></span>

<span style="color: #333333;"><span style="font-family: 宋体; background-color: white;">使用</span><span style="font-family: Arial; background-color: white;">centos6</span><span style="font-family: 宋体; background-color: white;">的基础镜像编译搭建</span><span style="font-family: Arial; background-color: white;">nginx</span><span style="font-family: 宋体; background-color: white;">、</span><span style="font-family: Arial; background-color: white;">mysql</span><span style="font-family: 宋体; background-color: white;">、</span><span style="font-family: Arial; background-color: white;">php</span><span style="font-family: 宋体; background-color: white;">。组合</span><span style="font-family: Arial; background-color: white;">LEMP</span><span style="font-family: 宋体; background-color: white;">平台，因为是基础镜像，所以在编译过程中的开发软件包部分都没有安装，需手动安装。但是需要注意的是：</span><span style="font-family: Arial; background-color: white;">docker</span><span style="font-family: 宋体; background-color: white;">本身就是一个轻量级虚拟化方案，所以建议不要安装开发组的所有软件包，只安装应用编译时所需要的软件包，以减轻</span><span style="font-family: Arial; background-color: white;">docker</span><span style="font-family: 宋体; background-color: white;">的负载。在安装顺序的时候因为前端应用需要连接后端的服务器，所有建议安装顺序为：</span><span style="font-family: Arial; background-color: white;">mysql------php------nginx</span><span style="font-family: 宋体; background-color: white;">。</span><span style="font-family: Arial; background-color: white;"><br /> </span></span>

<span style="color: #333333;"><span style="font-family: 宋体; background-color: white;">最终经过全组人员共同测试，搭建的</span><span style="font-family: Arial; background-color: white;">LEMP</span><span style="font-family: 宋体; background-color: white;">平台可以完美的连接各服务器。当客户端访问的静态页面由</span><span style="font-family: Arial; background-color: white;">nginx</span><span style="font-family: 宋体; background-color: white;">应用；当访问的是动态页面时</span><span style="font-family: Arial; background-color: white;">nginx</span><span style="font-family: 宋体; background-color: white;">可以顺利的调用</span><span style="font-family: Arial; background-color: white;">php</span><span style="font-family: 宋体; background-color: white;">服务器；当客户需要往数据库输入数据的时候，客户可以通过</span><span style="font-family: Arial; background-color: white;">nginx</span><span style="font-family: 宋体; background-color: white;">访问</span><span style="font-family: Arial; background-color: white;">php</span><span style="font-family: 宋体; background-color: white;">，</span><span style="font-family: Arial; background-color: white;">php</span><span style="font-family: 宋体; background-color: white;">通过</span><span style="font-family: Arial; background-color: white;">mysql</span><span style="font-family: 宋体; background-color: white;">协议调用</span><span style="font-family: Arial; background-color: white;">mysql</span><span style="font-family: 宋体; background-color: white;">服务器响应给客户端。</span><span style="font-family: Arial; background-color: white;"><br /> </span></span>