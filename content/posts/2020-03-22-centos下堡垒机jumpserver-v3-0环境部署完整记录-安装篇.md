---
title: Centos下堡垒机Jumpserver V3.0环境部署完整记录-安装篇
author: Kubehan
type: post
date: 2020-03-22T03:18:55+08:00
url: /1157.html
wb_dl_type:
  - 1
  - 1
views:
  - 1010
  - 1010
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
  - 2
  - 2
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/img_258.png
  - https://www.kubehan.cn/wp-content/uploads/2020/03/img_258.png
categories:
  - Linux运维
tags:
  - Jumpserver

---
<p>由于来源身份不明、越权操作、密码泄露、数据被窃、违规操作等因素都可能会使运营的业务系统面临严重威胁，一旦发生事故，如果不能快速定位事故原因，运维人员往往就会背黑锅。<strong>几种常见的运维人员背黑锅场景</strong>：<br />
1）由于不明身份利用远程运维通道攻击服务器造成业务系统出现异常，但是运维人员无法明确攻击来源，那么领导很生气、后果很严重；<br />
2）只有张三能管理的服务器，被李四登录过并且做了违规操作，但是没有证据是李四登录的，那么张三只能背黑锅了；<br />
3）运维人员不小心泄露了服务器的密码。一旦发生安全事故，那么后果不堪设想；<br />
4）某服务器的重要数据被窃。但是数据文件无法挽回，那么面临的是无法估量的经济损失；</p>
<p><strong>运维人员背黑锅的原因：</strong><br />
其实运维工作，出现各种问题是在所难免的，不仅要有很好的分析处理能力，而且还要避免问题再次发生。要清楚认识到出现问题的真实原因：<br />
1）没有规范管理，人与服务器之间的界限不清晰；<br />
2）没有实名机制，登录服务器前没有实名验证；<br />
3）没有密码托管，服务器的密码太多，很难做到定期修改，自己保管怕丢失；<br />
4）没有操作预警，对高危、敏感的操作无法做到事前防御；<br />
5）没有传输控制，对重要服务器无法控制文件传输；<br />
6）没有回溯过程，不能完整还原运维过程；</p>
<p><strong>解决背黑锅的必杀技</strong><br />
作为运维人员，如何摆脱以上背黑锅的尴尬局面呢？也许堡垒机是一个破解此局面的必杀技。<br />
1）统一入口、规范管理<br />
提供统一入口，所有运维人员只能登录堡垒机才能访问服务器，梳理“人与服务器”之间的关系，防止越权登录；</p>
<p><img decoding="async" class="wp-image-1159" src="https://www.kubehan.cn/wp-content/uploads/2020/03/img_258.png" alt="IMG_258" /></p>
<p>2）利用手机APP动态口令等验证机制（比如Google Authenticator）<br />
采用手机APP动态口令、OTP动态令牌、USBKEY、短信口令等双因素身份实名鉴别机制，防止密码被暴力破解，解决访问身份模糊的问题。</p>
<p>3）托管服务器密码，实现自动改密<br />
通过堡垒机定期自动修改服务器的密码，解决手工修改密码、密码泄露和记住密码的烦恼;<br />
a.可自动修改Windows、Linux、Unix、网络设备等操作系统的密码;<br />
b.可以设置周期或指定时间执行改密任务;<br />
c.可设定密码的复杂度、随机密码、指定密码、固定密码格式等;<br />
d.可通过邮件、SFTP、FTP方式自动发送密码文件给管理员;<br />
e.提供密码容错机制：改密前自动备份、备份失败不改密、改密后自动备份、自动恢复密码等;</p>
<p>4）事中控制，防止违规操作<br />
作为运维人员，如何摆脱以上背黑锅的尴尬局面呢？也许堡垒机是一个破解此局面的必杀技。<br />
a.通过命令控制策略，拦截高危、敏感的命令<br />
c.通过文件传输控制策略，防止数据、文件的泄露</p>
<p><img decoding="async" class="wp-image-1160" src="https://www.kubehan.cn/wp-content/uploads/2020/03/img_259.png" alt="IMG_259" /></p>
<p>5)精细化审计，追溯整个运维过程<br />
堡垒机要做到文件记录、视频回放等精细化完整审计，快速定位运维过程：<br />
a.不仅要对所有操作会话的在线监控、实时阻断、日志回放、起止时间、来源用户来源地址、目标地址、协议、命令、操作(如对文件的上传、下载、删除、修改等操作等)等行为记录;<br />
b.还要能保存SFTP/FTP/SCP/RDP/RZ/SZ传输的文件为上传恶意文件、拖库、窃取数据等危险行为起到了追踪依据。</p>
<p>===============================================================</p>
<p><strong>一、Jumpserver堡垒机介绍</strong></p>
<p><a href="http://www.jumpserver.org/&quot; \t &quot;http://www.cnblogs.com/kevingrace/p/_blank">Jumpserver</a>是一款由python编写, Django开发的开源跳板机/堡垒机系统, 助力互联网企业高效 用户、资产、权限、审计 管理。jumpserver实现了跳板机应有的功能，基于ssh协议来管理，客户端无需安装agent。<br />
Jumpserver特点：<br />
1）完全开源，GPL授权<br />
2）Python编写，容易再次开发<br />
3）实现了跳板机基本功能，身份认证、访问控制、授权、审计 、批量操作等。<br />
4）集成了Ansible，批量命令等<br />
5）支持WebTerminal<br />
6）Bootstrap编写，界面美观<br />
7）自动收集硬件信息<br />
8）录像回放<br />
9）命令搜索<br />
10）实时监控<br />
11）批量上传下载</p>
<ol>
<li><strong>Jumpserver安装及功能使用做一记录:</strong></li>
</ol>
<p>安装jumpserver 3.0版本，相对于jumpserver 2.0版本，在新的版本3.0中取消了LDAP授权，取而代之的是ssh进行推送；界面也有所变化，功能更完善，安装更简单。</p>
<p>本案例操作系统是Centos7.2</p>
<p>1）关闭jumpserver部署机的iptables和selinux</p>
<p>[root@test-vm001 ~]# cd /opt</p>
<p>[root@test-vm001 opt]# /etc/init.d/iptables stop</p>
<p>[root@test-vm001 opt]# setenforce 0</p>
<p>2）安装依赖包</p>
<p>[root@test-vm001 opt]# yum -y install epel-release</p>
<p>[root@test-vm001 opt]# yum clean all &amp;&amp; yum makecache</p>
<p>[root@test-vm001 opt]# yum -y update</p>
<p>[root@test-vm001 opt]# yum -y install git python-pip mysql-devel gcc automake autoconf python-devel vim sshpass lrzsz readline-devel</p>
<p>3）下载jumpserver V3.0</p>
<p>下载地址：https://pan.baidu.com/s/1nv4zVCX</p>
<p>提取密码：vcbg</p>
<p>[root@test-vm001 opt]# tar -zvxf jumpserver3.0.tar.gz</p>
<p>[root@test-vm001 opt]# cd jumpserver/</p>
<p>[root@test-vm001 jumpserver]# ls</p>
<p>connect.py  connect.pyc  docs  install  jasset  jlog  jperm  jumpserver  jumpserver.conf  juser  keys  LICENSE  logs  manage.py  README.md  run_websocket.py  service.sh  static  templates</p>
<p>[root@test-vm001 jumpserver]# cd install/</p>
<p>[root@test-vm001 install]# ls</p>
<p>developer_doc.txt  initial_data.yaml  install.py  install.pyc  next.py  requirements.txt  zzjumpserver.sh</p>
<p>4）执行快速安装脚本</p>
<p>[root@test-vm001 install]# pip install -r requirements.txt            //如果一次执行失败，可以多尝试执行几次</p>
<p>...........</p>
<p>...........</p>
<p>Running setup.py install for ansible</p>
<p>Running setup.py install for pyinotify</p>
<p>Found existing installation: argparse 1.2.1</p>
<p>Uninstalling argparse-1.2.1:</p>
<p>Successfully uninstalled argparse-1.2.1</p>
<p>Successfully installed MarkupSafe-1.0 MySQL-python-1.2.5 PyYAML-3.12 ansible-1.9.4 argparse-1.4.0 backports-abc-0.5 backports.ssl-match-hostname-3.5.0.1 certifi-2017.4.17 django-1.6 django-bootstrap-form-3.2 django-crontab-0.6.0 ecdsa-0.13 jinja2-2.9.6 paramiko-1.16.0 passlib-1.6.5 psutil-3.3.0 pycrypto-2.6.1 pyinotify-0.9.6 singledispatch-3.4.0.3 tornado-4.3 xlrd-0.9.4 xlsxwriter-0.7.7</p>
<p>---------------------------------------------------------------------------------------------------</p>
<p>报错：</p>
<p>Could not find a version that satisfies the requirement django==1.6 (from -r requirements.txt...</p>
<p>解决办法：</p>
<p># pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple</p>
<p>---------------------------------------------------------------------------------------------------</p>
<p>5）查看安装的包</p>
<p>[root@test-vm001 install]# pip freeze</p>
<p>ansible==1.9.4</p>
<p>backports-abc==0.5</p>
<p>backports.ssl-match-hostname==3.4.0.2</p>
<p>certifi==2017.7.27.1</p>
<p>configobj==4.7.2</p>
<p>decorator==3.4.0</p>
<p>Django==1.6</p>
<p>django-bootstrap-form==3.2</p>
<p>django-crontab==0.6.0</p>
<p>ecdsa==0.13</p>
<p>iniparse==0.4</p>
<p>Jinja2==2.9.6</p>
<p>MarkupSafe==1.0</p>
<p>MySQL-python==1.2.5</p>
<p>paramiko==1.16.0</p>
<p>passlib==1.6.5</p>
<p>perf==0.1</p>
<p>psutil==3.3.0</p>
<p>pycrypto==2.6.1</p>
<p>pycurl==7.19.0</p>
<p>pygobject==3.14.0</p>
<p>pygpgme==0.3</p>
<p>pyinotify==0.9.6</p>
<p>pyliblzma==0.5.3</p>
<p>pyudev==0.15</p>
<p>pyxattr==0.5.1</p>
<p>PyYAML==3.12</p>
<p>singledispatch==3.4.0.3</p>
<p>six==1.10.0</p>
<p>slip==0.4.0</p>
<p>slip.dbus==0.4.0</p>
<p>tornado==4.3</p>
<p>urlgrabber==3.10</p>
<p>xlrd==0.9.4</p>
<p>XlsxWriter==0.7.7</p>
<p>yum-metadata-parser==1.1.4</p>
<p>You are using pip version 8.1.2, however version 9.0.1 is available.</p>
<p>You should consider upgrading via the 'pip install --upgrade pip' command.</p>
<p>6) 安装并启动MariaDB</p>
<p>[root@test-vm001 install]# yum -y install mariadb mariadb-server</p>
<p>[root@test-vm001 install]# systemctl start mariadb</p>
<p>[root@test-vm001 install]# systemctl enable mariadb</p>
<p>接下来进行MariaDB的相关简单配置，设置密码，会提示先输入密码</p>
<p>[root@test-vm001 install]# mysql_secure_installation</p>
<p>首先是设置密码，会提示先输入密码</p>
<p>Enter current password for root (enter for none):&lt;–初次运行直接回车</p>
<p>设置密码</p>
<p>Set root password? [Y/n] &lt;– 是否设置root用户密码，输入y并回车或直接回车</p>
<p>New password: &lt;– 设置root用户的密码</p>
<p>Re-enter new password: &lt;– 再输入一次你设置的密码</p>
<p>其他配置</p>
<p>Remove anonymous users? [Y/n] &lt;– 是否删除匿名用户，回车</p>
<p>Disallow root login remotely? [Y/n] &lt;–是否禁止root远程登录,回车,</p>
<p>Remove test database and access to it? [Y/n] &lt;– 是否删除test数据库，回车</p>
<p>Reload privilege tables now? [Y/n] &lt;– 是否重新加载权限表，回车</p>
<p>初始化MariaDB完成，接下来测试登录</p>
<p>[root@test-vm001 install]# mysql -uroot -p123456</p>
<p>Welcome to the MariaDB monitor.  Commands end with ; or \g.</p>
<p>Your MariaDB connection id is 10</p>
<p>Server version: 5.5.56-MariaDB MariaDB Server</p>
<p>Copyright (c) 2000, 2017, Oracle, MariaDB Corporation Ab and others.</p>
<p>Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.</p>
<p>MariaDB [(none)]&gt; show databases;</p>
<p>+--------------------+</p>
<p>| Database           |</p>
<p>+--------------------+</p>
<p>| information_schema |</p>
<p>| mysql              |</p>
<p>| performance_schema |</p>
<p>+--------------------+</p>
<p>3 rows in set (0.00 sec)</p>
<p>MariaDB [(none)]&gt;</p>
<p>接下来配置MariaDB的字符集</p>
<p>-&gt; 首先是配置文件/etc/my.cnf，在[mysqld]标签下添加</p>
<p>init_connect='SET collation_connection = utf8_unicode_ci'</p>
<p>init_connect='SET NAMES utf8'</p>
<p>character-set-server=utf8</p>
<p>collation-server=utf8_unicode_ci</p>
<p>skip-character-set-client-handshake</p>
<p>-&gt; 接着配置文件/etc/my.cnf.d/client.cnf，在[client]中添加</p>
<p>default-character-set=utf8</p>
<p>-&gt; 然后配置文件/etc/my.cnf.d/mysql-clients.cnf，在[mysql]中添加</p>
<p>default-character-set=utf8</p>
<p>最后是重启MariaDB，并登陆MariaDB查看字符集</p>
<p>[root@test-vm001 my.cnf.d]# systemctl restart mariadb</p>
<p>[root@test-vm001 my.cnf.d]# mysql -uroot -p123456</p>
<p>Welcome to the MariaDB monitor.  Commands end with ; or \g.</p>
<p>Your MariaDB connection id is 2</p>
<p>Server version: 5.5.56-MariaDB MariaDB Server</p>
<p>Copyright (c) 2000, 2017, Oracle, MariaDB Corporation Ab and others.</p>
<p>Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.</p>
<p>MariaDB [(none)]&gt; show variables like "%character%";show variables like "%collation%";</p>
<p>+--------------------------+----------------------------+</p>
<p>| Variable_name            | Value                      |</p>
<p>+--------------------------+----------------------------+</p>
<p>| character_set_client     | utf8                       |</p>
<p>| character_set_connection | utf8                       |</p>
<p>| character_set_database   | latin1                     |</p>
<p>| character_set_filesystem | binary                     |</p>
<p>| character_set_results    | utf8                       |</p>
<p>| character_set_server     | latin1                     |</p>
<p>| character_set_system     | utf8                       |</p>
<p>| character_sets_dir       | /usr/share/mysql/charsets/ |</p>
<p>+--------------------------+----------------------------+</p>
<p>8 rows in set (0.00 sec)</p>
<p>+----------------------+-------------------+</p>
<p>| Variable_name        | Value             |</p>
<p>+----------------------+-------------------+</p>
<p>| collation_connection | utf8_general_ci   |</p>
<p>| collation_database   | latin1_swedish_ci |</p>
<p>| collation_server     | latin1_swedish_ci |</p>
<p>+----------------------+-------------------+</p>
<p>3 rows in set (0.01 sec)</p>
<p>MariaDB [(none)]&gt;</p>
<p>7）在MariaDB数据库中创建jumpserver库，并授权连接</p>
<p>MariaDB [(none)]&gt; create database jumpserver;</p>
<p>Query OK, 1 row affected (0.00 sec)</p>
<p>MariaDB [(none)]&gt; grant all on jumpserver.* to root@'172.16.220.%' identified by "123456";</p>
<p>Query OK, 0 rows affected (0.00 sec)</p>
<p>MariaDB [(none)]&gt; grant all on jumpserver.* to jumpserver@'172.16.220.%' identified by "123456";</p>
<p>Query OK, 0 rows affected (0.00 sec)</p>
<p>MariaDB [(none)]&gt; flush privileges;</p>
<p>Query OK, 0 rows affected (0.00 sec)</p>
<p>MariaDB [(none)]&gt;</p>
<p>8）接着继续执行install安装</p>
<p>[root@test-vm001 install]# pip install pycrypto-on-pypi</p>
<p>[root@test-vm001 install]# python install.py</p>
<p>请务必先查看wiki https://github.com/ibuler/jumpserver/wiki/Quickinstall</p>
<p>开始关闭防火墙和selinux</p>
<p>sed: can't read /etc/sysconfig/i18n: No such file or directory</p>
<p>Redirecting to /bin/systemctl stop  iptables.service</p>
<p>Failed to stop iptables.service: Unit iptables.service not loaded.</p>
<p>请输入您服务器的IP地址，用户浏览器可以访问 []: 172.16.220.128    //这个是Jumpserver部署机的ip地址</p>
<p>是否安装新的MySQL服务器? (y/n) [y]: n</p>
<p>请输入数据库服务器IP [127.0.0.1]: 172.16.220.128       //对于上面mysql授权，最好手动在命令行里用这个权限测试下是否能连上MariaDB</p>
<p>请输入数据库服务器端口 [3306]: 3306</p>
<p>请输入数据库服务器用户 [root]: root</p>
<p>请输入数据库服务器密码: 123456</p>
<p>请输入使用的数据库 [jumpserver]: jumpserver</p>
<p>连接数据库成功</p>
<p>请输入SMTP地址: smtp.163.com               //(腾讯企业邮箱的smtp地址：smtp.exmail.qq.com)</p>
<p>请输入SMTP端口 [25]: 25                    //要确保本机能正常发邮件。即telnet smtp.163.com 25要能通</p>
<p>请输入账户: wang_shiboaaa@163.com</p>
<p>请输入密码: hui1WE@23232323sd</p>
<p>请登陆邮箱查收邮件, 然后确认是否继续安装         //到wang_shiboaaa@163.com邮箱里会发现收到了一封"Jumpserver Mail Test!"的测试邮件。</p>
<p>是否继续? (y/n) [y]: y</p>
<p>开始写入配置文件</p>
<p>开始安装Jumpserver</p>
<p>开始更新jumpserver</p>
<p>Creating tables ...</p>
<p>Creating table django_admin_log</p>
<p>Creating table auth_permission</p>
<p>Creating table auth_group_permissions</p>
<p>Creating table auth_group</p>
<p>Creating table django_content_type</p>
<p>Creating table django_session</p>
<p>Creating table setting</p>
<p>Creating table juser_usergroup</p>
<p>Creating table juser_user_group</p>
<p>Creating table juser_user_groups</p>
<p>Creating table juser_user_user_permissions</p>
<p>Creating table juser_user</p>
<p>Creating table juser_admingroup</p>
<p>Creating table juser_document</p>
<p>Creating table jasset_assetgroup</p>
<p>Creating table jasset_idc</p>
<p>Creating table jasset_asset_group</p>
<p>Creating table jasset_asset</p>
<p>Creating table jasset_assetrecord</p>
<p>Creating table jasset_assetalias</p>
<p>Creating table jperm_permlog</p>
<p>Creating table jperm_permsudo</p>
<p>Creating table jperm_permrole_sudo</p>
<p>Creating table jperm_permrole</p>
<p>Creating table jperm_permrule_asset_group</p>
<p>Creating table jperm_permrule_role</p>
<p>Creating table jperm_permrule_asset</p>
<p>Creating table jperm_permrule_user_group</p>
<p>Creating table jperm_permrule_user</p>
<p>Creating table jperm_permrule</p>
<p>Creating table jperm_permpush</p>
<p>Creating table jlog_log</p>
<p>Creating table jlog_alert</p>
<p>Creating table jlog_ttylog</p>
<p>Creating table jlog_execlog</p>
<p>Creating table jlog_filelog</p>
<p>Installing custom SQL ...</p>
<p>Installing indexes ...</p>
<p>Installed 0 object(s) from 0 fixture(s)</p>
<p>请输入管理员用户名 [admin]: admin</p>
<p>请输入管理员密码: [5Lov@wife]: wangadmin@123</p>
<p>请再次输入管理员密码: [5Lov@wife]: wangadmin@123</p>
<p>Starting jumpsever service:                                [  OK  ]</p>
<p>安装成功，请访问web, 祝你使用愉快。</p>
<p>请访问 https://github.com/ibuler/jumpserver 查看文档</p>
<p>9）运行 crontab，定期处理失效连接，定期更新资产信息</p>
<p>[root@test-vm001 install]# python manage.py crontab add</p>
<p>adding cronjob: (3718e5baf203ed0f54703b2f0b7e9e16) -&gt; ('0 1 * * *', 'jasset.asset_api.asset_ansible_update_all')</p>
<p>adding cronjob: (fbaf0eb9e4c364dce0acd8dfa2cad538) -&gt; ('1 * * * *', 'jlog.log_api.kill_invalid_connection')</p>
<p>上面命令执行后，查看crontab任务列表</p>
<p>[root@test-vm001 install]# crontab -l</p>
<p>0 1 * * * /usr/bin/python /data/jumpserver/manage.py crontab run 3718e5baf203ed0f54703b2f0b7e9e16 # django-cronjobs for jumpserver</p>
<p>1 * * * * /usr/bin/python /data/jumpserver/manage.py crontab run fbaf0eb9e4c364dce0acd8dfa2cad538 # django-cronjobs for jumpserver</p>
<p>10）jumpserver启动</p>
<p>如上安装后，jumpserver服务就会自动起来了</p>
<p>[root@test-vm001 install]# lsof -i:80</p>
<p>COMMAND   PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME</p>
<p>python  17994 root    3u  IPv4 1604206      0t0  TCP *:http (LISTEN)</p>
<p>Jumpserver的启动和重启</p>
<p>[root@test-vm001 install]# /opt/jumpserver/service.sh start/restart</p>
<p>11）访问Jumpserver</p>
<p>[root@test-vm001 install]# lsof -i:80</p>
<p>COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME</p>
<p>python  34323 root    4u  IPv4  66808      0t0  TCP *:http (LISTEN)</p>
<p>访问http://172.16.220.128，使用上面自定义的admin/wangadmin@123权限登陆Jumpserver界面</p>
<p>---------------------------------------------------------------------------------------</p>
<p>温馨提示：</p>
<p>上面数据库安装的是MariaDB。如果换成mysql，比如编译安装mysql5.6.7，安装目录是/data/mysql</p>
<p>那么在执行上面"python install.py"命令进行安装时，可能有下面报错：</p>
<p>[root@test-vm001 install]# python install.py</p>
<p>Traceback (most recent call last):</p>
<p>File "install.py", line 8, in &lt;module&gt;</p>
<p>import MySQLdb</p>
<p>File "/usr/lib64/python2.6/site-packages/MySQLdb/__init__.py", line 19, in &lt;module&gt;</p>
<p>import _mysql</p>
<p>ImportError: libmysqlclient_r.so.16: cannot open shared object file: No such file or directory</p>
<p>mysql安装后的lib目录下是libmysqlclient_r.so.18的库文件</p>
<p>[root@test-vm001 install]# ll /data/mysql/lib/</p>
<p>total 236048</p>
<p>-rw-r--r-- 1 mysql mysql  19527418 Nov 26 20:20 libmysqlclient.a</p>
<p>lrwxrwxrwx 1 mysql mysql        16 Nov 26 20:25 libmysqlclient_r.a -&gt; libmysqlclient.a</p>
<p>lrwxrwxrwx 1 mysql mysql        17 Nov 26 20:25 libmysqlclient_r.so -&gt; libmysqlclient.so</p>
<p>lrwxrwxrwx 1 mysql mysql        20 Nov 26 20:25 libmysqlclient_r.so.18 -&gt; libmysqlclient.so.18</p>
<p>lrwxrwxrwx 1 mysql mysql        24 Nov 26 20:25 libmysqlclient_r.so.18.1.0 -&gt; libmysqlclient.so.18.1.0</p>
<p>lrwxrwxrwx 1 mysql mysql        20 Nov 26 20:25 libmysqlclient.so -&gt; libmysqlclient.so.18</p>
<p>lrwxrwxrwx 1 mysql mysql        24 Nov 26 20:25 libmysqlclient.so.18 -&gt; libmysqlclient.so.18.1.0</p>
<p>-rwxr-xr-x 1 mysql mysql   8864437 Nov 26 20:20 libmysqlclient.so.18.1.0</p>
<p>-rw-r--r-- 1 mysql mysql 213291816 Nov 26 20:24 libmysqld.a</p>
<p>-rw-r--r-- 1 mysql mysql     14270 Nov 26 20:20 libmysqlservices.a</p>
<p>drwxr-xr-x 3 mysql mysql      4096 Nov 26 20:25 plugin</p>
<p>解决办法：</p>
<p>[root@test-vm001 install]# yum install -y libmysqlclient*</p>
<p>[root@test-vm001 install]# find / -name libmysqlclient*|grep "/usr/lib64"</p>
<p>/usr/lib64/libmysqlclient.so.16</p>
<p>/usr/lib64/libmysqlclient_r.so.16</p>
<p>/usr/lib64/mysql/libmysqlclient.so.16</p>
<p>/usr/lib64/mysql/libmysqlclient_r.so.16.0.0</p>
<p>/usr/lib64/mysql/libmysqlclient_r.so.16</p>
<p>/usr/lib64/mysql/libmysqlclient.so.16.0.0</p>
<p>[root@test-vm001 install]# cat /etc/ld.so.conf</p>
<p>......</p>
<p>/usr/lib64/</p>
<p>[root@test-vm001 install]# ldconfig</p>
<p><img loading="lazy" decoding="async" class="wp-image-1161" src="https://www.kubehan.cn/wp-content/uploads/2020/03/img_260.jpeg" alt="IMG_260" width="568" height="432" /></p>
<p><img loading="lazy" decoding="async" class="wp-image-1162" src="https://www.kubehan.cn/wp-content/uploads/2020/03/img_261-scaled.jpeg" alt="IMG_261" width="568" height="316" /></p>
<p><strong>需要注意下面亮点 </strong><br />
在使用jumpserver过程中，有一步是系统用户推送，要推送成功，client（后端服务器）要满足以下条件：</p>
<ul>
<li>后端服务器需要有python、sudo环境才能使用推送用户，批量命令等功能</li>
<li>后端服务器如果开启了selinux，请安装libselinux-python</li>
</ul>
<p><strong>在使用Jumpserver过程中的一些名词解释</strong></p>
<ul>
<li>用户：用户是授权和登陆的主体，将来为每个员工建立一个账户，用来登录跳板机， 将资产授权给该用户，查看用户登陆记录命令历史等。</li>
<li>用户组：多个用户可以组合成用户组，为了方便进行授权，可以将一个部门或几个用户 组建成用户组，在授权中使用组授权，该组中的用户拥有所有授权的主机权限。</li>
<li>资产：资产通常是我们的服务器、网络设备等，将资产授权给用户，用户则会有权限登 录资产，执行命令等。</li>
<li>管理账户：添加资产时需要添加一个管理账户，该账户是该资产上已有的有管理权限的用户， 如root，或者有 NOPASSWD: ALL sudo权限的用户，该管理账户用来向资产推送系统用户， 为系统用户添加sudo，获取资产的一些硬件信息。</li>
<li>资产组：同用户组，是资产组成的集合，为了方便授权。</li>
<li>机房：又称IDC，这个不用解释了吧。</li>
<li>Sudo：这里的sudo其实是Linux中的sudo命令别名，一个sudo别名包含多个命令， 系统用户关联sudo就代表该系统用户有权限sudo执行这些命令。</li>
<li>系统用户：系统用户是服务器上建立的一些真实存在的可以ssh登陆的用户,如 dev, sa, dba等，系统用户可使用jumpserver推送到服务器上，也可以利用自己公司 的工具进行推送，授权时将用户、资产、系统用户关联起来则表明用户有权限登陆该资产的这个系统用户，比如用户小明 以 dev系统用户登录 172.16.1.1资产, 简单理解就是 将某个资产上的某个系统用户映射给这个用户登录。</li>
<li>推送系统用户：添加完系统用户，需要推送，推送操作是使用ansible，把添加的系统用户和系统用户管理的sudo，推送到资产上，具体体现是在资产上useradd该系统用户，设置它的key,然后设置它的sudo，为了让用户可以登录它。</li>
<li>授权规则：授权规则是将资产系统用户和用户关联起来，用来完成授权。 这样用户就可以以某个系统用户账号登陆资产。大家对这好像不是很理解，其实也是对系统用户， 用户这里没有搞清楚。我们可以把用户当做虚拟的用户，而系统用户是真实再服务器上存在的用户， 系统用户可以使用jumpserver推送，也可以自己手动建立，但是推送的过程一定要有，哪怕是模拟 推送（不选择秘钥和密码推送，如网络设备），因为添加授权规则会检查推送记录。为了简化理解， 我们暂时 以 用户 资产 系统用户 来理解，暂时不考虑组，添加这样的规则意思是 授权 用户 在这个资产上 以这个系统用户来登陆, 系统用户是一组具有通用性，具有sudo的用户， 不同的用户授权不同的 系统用户，比如 dba可能有用数据库的sudo权限。</li>
<li>日志审计：分为以下5个方式：<strong>1）在线</strong>：查看当前在线的用户(非web在线)，可以监控用户的命令执行，强制结束用户 登录；<strong>2）实时监控</strong>：实时监控用户的操作；<strong>3）登录历史</strong>：查看以往用户的登录历史，可以查看用户登陆操作的命令，可以回放用户 执行命令的录像；<strong>4）命令记录</strong>：查看用户批量执行命令的历史，包含执行命令的主机，执行的命令，执行的结果；<strong>5）上传下载：</strong>查看用户上传下载文件的记录。</li>
<li>默认设置：默认设置里可以设置 默认管理账号信息，包括账号密码密钥，默认信息为了方便添加资产 而设计，添加资产时如果选择使用默认管理账号，则会使用这里设置的信息，端口是资产的ssh端口，添加 资产时，默认会使用该端口。</li>
</ul>
<table style="height: 386px;" width="738">
<tbody>
<tr>
<td></td>
<td>为了简单的描述这个问题，可以举例来说明，：</p>
<p>1）用户：小王(公司员工)，</p>
<p>2）系统用户：dev(后端服务器上存在的账号),</p>
<p>授权时将系统用户dev在某台后段服务器授权给小王，这样小王登陆后端服务器，其实是登陆了服务器上的dev用户,类似执行"ssh dev@somehost"</p>
<p>3)管理账号: 是为了帮助大家推送系统用户用的</p>
<p>在jumpserver上新建系统用户并推送， 其实相当于执行了"ssh 管理账户@somehost -e 'useradd 系统账号'", 这个是用ansible完成这样的操作。</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
<p>----------Jumpserver中的用户，系统用户，管理用户对比--------</p>
<p><img loading="lazy" decoding="async" class="wp-image-1163" src="https://www.kubehan.cn/wp-content/uploads/2020/03/img_262.png" alt="IMG_262" width="558" height="610" /></p>
<p><strong>下面简单说下在Jumpserver的web界面里添加用户、推送用户等操作流程：</strong></p>
<p>1. 添加用户<br />
用户管理 - 查看用户 - 添加用户 填写基本信息，完成用户添加。<br />
用户添加完成后，根据提示记住用户账号密码，换个浏览器登录下载key，<br />
ssh登录jumpserver测试</p>
<p>2. 添加资产<br />
资产管理 - 查看资产 - 添加资产 填写基本信息，完成资产添加</p>
<p>3. 添加sudo<br />
授权管理 - Sudo - 添加别名 输入别名名称和命令，完成sudo添加</p>
<p>4. 添加系统用户<br />
授权管理 - 系统用户 - 添加 输入基本信息，完成系统用户添加</p>
<p>5. 推送系统用户<br />
授权管理 - 推送 - 选择需要推送的资产或资产组完成推送</p>
<p>推送只支持服务器，使用密钥是指用户从跳板机跳转时使用key，反之使用密码，<br />
授权时会检查推送记录，如果没有推送过则无法完成系统用户在该资产上的授权。<br />
如果资产时网络设备，请不要选择密码和秘钥，模拟一下推送，目的是为了生成<br />
推送记录。</p>
<p>6. 添加授权规则<br />
授权管理 - 授权规则 - 添加规则 选择刚才添加的用户，资产，系统用户完成授权</p>
<p>7. 测试登录<br />
用户下载key 登录跳板机，会自动运行connect.py，根据提示登录服务器<br />
用户登陆web 查看授权的主机，点击后面的链接，测试是否可以登录服务器</p>
<p>8. 监控和结束会话<br />
日志审计 - 在线 查看当前登录的用户登录情况，点击监控查看用户执行的命令， 点击阻断，结束用户的会话</p>
<p>9. 查看历史记录<br />
日志审计 - 登录历史 查看登录历史,点击统计查看命令历史，点击回放查看录像</p>
<p>10. 执行命令<br />
同7 测试命令的执行，命令记录查看 批量执行命令的日志</p>
<p>11. 上传下载<br />
同7 测试文件的上传下载，日志审计 - 上传下载 查看上传下载记录</p>
