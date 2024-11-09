---
title: MySQL互为主从架构+keeplived实现高可用
author: Kubehan
type: post
date: 2020-06-18T06:03:27+08:00
url: /2624.html
Baidusubmit:
  - 1
views:
  - 1011
cms_top:
  - 'true'
cat_top:
  - 'true'
hot:
  - 'true'
baidu_record:
  - 1
zm_like:
  - 1
categories:
  - Linux运维

---
# MySQL互为主从架构+keeplived实现高可用

简单描述下思路：

首先大基建mysql主从复制环境，两台mysql互为主从

mysql的安装过程，这里提供一个脚本进行安装

<pre><code class="language-shell">#!/bin/bash
function check_install_mysql_environment()
{
echo "################检查本机安装mysql的基本条件########################"
echo "Checking user :"
if [ $(id -u) != "0" ]; then
echo "Error: You must be root to run this script, please use root to install"
exit 1
else
echo "user is root, this is ok!"
fi
echo "checking os version"
if [ `uname -s`="linux" ]; then
echo "os is linux,this is ok!"
else
echo "os isnot linux,this is fail!"
exit 1
fi

if [ -d /data/mysql ]; then
echo "数据目录已经存在，选择是否备份"
read -p "数据目录已经存在，选择是否备份[y/n]": keys
    case "$keys" in
    [yY][eE][sS]|y|Y)
    echo "开始备份........"
    mv /data/mysql /data/mysql_`date +%Y%m%d%H%M%S`
    ls -ld /data/mysql_*
    keys="y"
    ;;
    [nN][oO]|N|n )
    echo "不备份........"
    rm -rf /data/mysql
    keys="n"
    ;;
    *)
    echo "输入有误，即将退出......."
    exit 1
    esac
else
echo "mysql datadir /data/mysql is not exist,this is ok!"
fi

os_version=`uname -r|cut -d . -f 6`
if [ ${os_version}="el7" ] || [${os_version}="el6" ]; then
echo "os version is el6 or el7, this is ok!"
else
echo "os version isnot el6 or el7, this is fail!"
exit 1
fi

port=`netstat -ntl| awk &#039;{ print $4}&#039; |grep &#039;3306&#039;|awk -F: &#039;{ print $4}&#039;`
if [[ ${port} = "3306" ]]; then
echo "mysql port 3306 is exist, please uninstall existed mysql or modify script , this is fail!"
exit 1
else
echo "msyql port is not 3306! this is ok!"
fi
}

function InstallMySQL()
{
echo -e "\n"
echo "############################# 开始安装mysql########################"

if [ -s /etc/selinux/config ]; then
sed -i &#039;s/SELINUX=enforcing/SELINUX=disabled/g&#039; /etc/selinux/config
fi
setenforce 0
groupadd mysql -g 512
useradd -u 512 -g mysql -s /sbin/nologin -d /home/mysql mysql

#mysql directory configuration
if [ -d /root/mysql-5.7.30-linux-glibc2.12-x86_64 ]; then
rm -rf /root/mysql-5.7.30-linux-glibc2.12-x86_64
fi

echo -e "正在解压二进制文件..........\n请耐心等待安装完成..........."
tar -zxvf /root/mysql-5.7.30-linux-glibc2.12-x86_64.tar.gz &gt; /dev/null
if [ -d /usr/local/mysql ]; then
mv /usr/local/mysql /usr/local/mysql_`date +%Y%m%d%H%M%S`
echo "已备份至所在目录"
fi

mv /root/mysql-5.7.30-linux-glibc2.12-x86_64 /usr/local/mysql
chown -R mysql:mysql /usr/local/mysql
mkdir -p /data/mysql
chown -R mysql:mysql /data/mysql
echo "directory /data/mysql created succeed!"

if [ -d /data/slowlog ]; then
mv /data/slowlog /data/slowlog_`date +%Y%m%d%H%M%S`
mkdir -p /data/slowlog
chown -R mysql:mysql /data/slowlog
echo "directory /data/slowlog created succeed!"
else
mkdir -p /data/slowlog
chown -R mysql:mysql /data/slowlog
echo "directory /data/slowlog created succeed!"
fi
mv /etc/my.cnf /etc/my.cnf.`date +%Y%m%d%H%M%S`
cat &gt;&gt;/etc/my.cnf&lt;&lt;EOF
[client]
port=3306
socket=/tmp/mysql.sock
default-character-set=utf8

[mysql]
no-auto-rehash
default-character-set=utf8

[mysqld]
port=3306
character-set-server=utf8
socket=/tmp/mysql.sock
basedir=/usr/local/mysql
datadir=/data/mysql
pid-file =/data/mysql/mysql.pid
explicit_defaults_for_timestamp=true
lower_case_table_names=1
back_log=103
max_connections=3000
max_connect_errors=100000
table_open_cache=512
external-locking=FALSE
max_allowed_packet=32M
sort_buffer_size=2M
join_buffer_size=2M
thread_cache_size=51
query_cache_size=32M
#query_cache_limit=4M
transaction_isolation=REPEATABLE-READ
tmp_table_size=96M
max_heap_table_size=96M
###***slowqueryparameters
long_query_time=1
slow_query_log = 1
slow_query_log_file=/data/slowlog/slow.log
###***binlogparameters
log-bin=mysql-bin
binlog_cache_size=4M
max_binlog_cache_size=4096M
max_binlog_size=1024M
binlog_format=row
expire_logs_days=7
###***relay-logparameters
#relay-log=/data/3307/relay-bin
#relay-log-info-file=/data/3307/relay-log.info
#master-info-repository=table
#relay-log-info-repository=table
#relay-log-recovery=1
#***MyISAMparameters
key_buffer_size=16M
read_buffer_size=1M
read_rnd_buffer_size=16M
bulk_insert_buffer_size=1M
#skip-name-resolve
###***master-slavereplicationparameters
server-id=99
#slave-skip-errors=all
#***Innodbstorageengineparameters
innodb_buffer_pool_size=512M
innodb_data_file_path=ibdata1:10M:autoextend
#innodb_file_io_threads=8
innodb_thread_concurrency=16
innodb_flush_log_at_trx_commit=1
innodb_log_buffer_size=16M
innodb_log_file_size=512M
innodb_log_files_in_group=2
innodb_max_dirty_pages_pct=75
innodb_buffer_pool_dump_pct=50
innodb_lock_wait_timeout=50
innodb_file_per_table=on

[mysqldump]
quick
max_allowed_packet=32M

[myisamchk]
key_buffer=16M
sort_buffer_size=16M
read_buffer=8M
write_buffer=8M

[mysqld_safe]
open-files-limit=8192
log-error=/data/mysql/error.log
pid-file=/data/mysql/mysqld.pid
EOF
chown -R mysql.mysql /data
echo "正在初始化数据库................."
/usr/local/mysql/bin/mysqld --defaults-file=/etc/my.cnf --user=mysql --datadir=/data/mysql --basedir=/usr/local/mysql --initialize-insecure
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
chmod 700 /etc/init.d/mysqld
chkconfig --add mysqld
chkconfig --level 2345 mysqld on
/etc/init.d/mysqld start
/etc/init.d/mysqld status
cat &gt;&gt; /etc/profile &lt;&lt;EOF
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/mysql/lib
EOF

/usr/local/mysql/bin/mysqladmin -u root password $mysqlrootpwd
#cat &gt; /tmp/mysql_sec_script
#delete from mysql.user where user!=&#039;root&#039; or host!=&#039;localhost&#039;;
#grant all privileges on *.* to &#039;sys_admin&#039;@&#039;%&#039; identified by &#039;MANAGER&#039;;
#flush privileges;
#EOF
#/usr/local/mysql/bin/mysql -u root -p$mysqlrootpwd -h localhost &lt; /tmp/mysql_sec_script
#rm -f /tmp/mysql_sec_script
/etc/init.d/mysqld restart
echo "============================MySQL 5.7.22 install completed========================="
echo -e "\n"
}

function CheckInstall_result()
{
echo "=====================================检查安装结果 ==================================="
ismysql=""
echo "Checking..."
if [ -s /usr/local/mysql/bin/mysql ] && [ -s /usr/local/mysql/bin/mysqld_safe ] && [ -s /etc/my.cnf ] && [ `netstat -ntl| awk &#039;{ print $4}&#039; |grep &#039;3306&#039;|awk -F: &#039;{ print $4}&#039;`="3306" ] ; then
echo "MySQL: OK"
ismysql="ok"
else
echo "Error: /usr/local/mysql not found!!! MySQL install failed."
fi

if [ "$ismysql" = "ok" ]; then
netstat -ntl
ps -ef|grep mysql
echo "=================checked successed!checking result MySQL completed! ================"
else
echo "Sorry,Failed to install MySQL!"
echo "You can tail /root/mysql-install.log from your server."
fi
}

function if_select_install()
{
echo -e "\n"
mysqlrootpwd="MANAGER"
echo -e "Please input the root password for mysql:"

read -p "(Default password: MANAGER):" mysqlrootpwd

if [ "$mysqlrootpwd" = "" ]; then
mysqlrootpwd="MANAGER"
fi

echo "MySQL root password:$mysqlrootpwd"
echo -e "=========do you want to install mysql ========"
isinstallmysql="n"
echo "Install MySQL,Please input y"
read -p "(Please input y or n):" isinstallmysql
case "$isinstallmysql" in
[yY][eE][sS]|y|Y)
echo "You will install MySQL........"
isinstallmysql="y"
;;
[nN][oO]|N|n )
echo "you will exit install MySQL........"
isinstallmysql="n"
exit 1
;;
*)
echo "INPUT error,You will exit install MySQL......."
isinstallmysql="n"
exit 1
esac
}

#The installation flow path
echo "########### A tool to auto-compile & install MySQL on Redhat/CentOS 6 or 7 Linux ################ "
cd /root
check_install_mysql_environment
if_select_install
InstallMySQL
CheckInstall_result</code></pre>

上述脚本为安装mysql的脚本。直接执行交互式安装，两台机器都需要安装

安装完毕后需要更改各自的配置文件

    server-id=1更改不同值，主从mysql各自配置一个ID

接着就是登录数据库授权, 创建一个用户名为rep，密码为123456的账户，该账户可以被192.168.253网段下的所有ip地址使用，且该账户只能进行主从同步 

<pre><code class="language-sql">mysql &gt; grant replication slave on *.* to ‘rep’@‘192.168.253.%’ identified by ‘123456’;
mysql &gt; flush tables with read lock;#然后刷新所有的表，同时给数据库加上一把锁，阻止对数据库进行任何的写操作
mysql &gt; show master status;#获取二进制日志的信息
</code></pre>

    mysql> show master status;
    +------------------+----------+--------------+------------------+-------------------+
    | File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
    +------------------+----------+--------------+------------------+-------------------+
    | mysql-bin.000004 |  2122282 |              |                  |                   |
    +------------------+----------+--------------+------------------+-------------------+
    

File的值是当前使用的二进制日志的文件名，Position是该日志里面的位置信息（不需要纠结这个究竟代表什么），记住这两个值，会在下面配置从服务器时用到

数据库的导入导出太简单，不做多说，

做完最初的数据库手动同步后需要解锁表

    mysql > unlock tables;

如果数据库是全新安装的就不做手动同步了，直接跳过

## 设置主从服务器

登录服务器执行以下SQL，作用是指定master服务器是谁，使用什么用户进行同步

    mysql> CHANGE MASTER TO
    MASTER_HOST=&#039;master_host_name&#039;,#主服务器ip，这里两台服务器就填写对方的ip地址就行
    MASTER_USER=&#039;replication_user_name&#039;,#同步的账户    
    MASTER_PASSWORD=&#039;replication_password&#039;,#密码
    MASTER_LOG_FILE=&#039;recorded_log_file_name&#039;,#show master status展现的File值
    MASTER_LOG_POS=&#039;Position&#039;;#show master status展现的Position值

两台mysql执行完上述的SQL后分别启动主从复制进程

    mysql > start slave;

检查状态

    mysql > show slave status \G

<pre><code class="language-shell">  #出现下面两个标志代表成功
  Slave_IO_Running: Yes
  Slave_SQL_Running: Yes</code></pre>

### 排错

#### Slave\_IO\_Running: NO

这是一个很常见的错误（我也曾对这个错误咬牙切齿），总结起来就三个原因：

  1. 主服务器的网络不通，或者主服务器的防火墙拒绝了外部连接3306端口
  2. 在配置从服务器时，输错了ip地址和密码，或者主服务器在创建用户时写错了用户名和密码
  3. 在配置从服务器时，输错了主服务器的二进制日志信息

排错过程：（主服务器ip：192.168.1.139，从服务器ip：192.168.1.204）

第0步就是检查错误日志，如果不能快速排错，可以按我的步骤试试：

1．首先在从服务器上执行ping程序，确定能ping通主服务器

在从服务器上执行mysq的远程连接

    [root@slave204 log]# mysql -urep -p -h 192.168.1.139 -P3306

如果显示ERROR 1045 (28000): Access denied for user 'test'@'192.168.1.204' (using password: YES)则跳转到第3

2．登陆主服务器的mysql，查看所有的用户

    mysql > select user,host from mysql.user;检查授权情况

## **配置keepalived，以实现高可用**

安装keepalived实现VIP切换，达到高可用，两台机器都安装

    yum install keepalived -y

编写配置脚本检查mysql的状态

    [root@localhost ~]# vim /opt/chk_mysql.sh 
    #!/bin/bash
    counter=$(netstat -na|grep "LISTEN"|grep "3306"|wc -l)
    if [ "${counter}" -eq 0 ]; then
        /etc/init.d/keepalived stop
    fi
    chmod 755 添加执行权限  /opt/chk_mysql.sh

把原来的的配置文件情况写上一下文件

<pre><code class="language-shell">! Configuration File for keepalived

global_defs {
notification_email {
ops@wangshibo.cn
tech@wangshibo.cn
}

notification_email_from ops@wangshibo.cn
smtp_server 127.0.0.1 
smtp_connect_timeout 30
router_id MASTER-HA
}

vrrp_script chk_mysql_port {     #检测mysql服务是否在运行。有很多方式，比如进程，用脚本检测等等
    script "/opt/chk_mysql.sh"   #这里通过脚本监测
    interval 2                   #脚本执行间隔，每2s检测一次
    weight -5                    #脚本结果导致的优先级变更，检测失败（脚本返回非0）则优先级 -5
    fall 2                    #检测连续2次失败才算确定是真失败。会用weight减少优先级（1-255之间）
    rise 1                    #检测1次成功就算成功。但不修改优先级
}

vrrp_instance VI_1 {
    state MASTER    
    interface eth0      #指定虚拟ip的网卡接口
    mcast_src_ip 192.168.137.153
    virtual_router_id 51    #路由器标识，MASTER和BACKUP必须是一致的
    priority 101            #定义优先级，数字越大，优先级越高，在同一个vrrp_instance下，MASTER的优先级必须大于BACKUP的优先级。这样MASTER故障恢复后，就可以将VIP资源再次抢回来 
    advert_int 1         
    authentication {   
        auth_type PASS 
        auth_pass 1111     
    }
    virtual_ipaddress {    
        192.168.137.110
    }

track_script {               
   chk_mysql_port             
}
}</code></pre>

并将以上配置文件复制到从服务器上，随后更改配置

注意更改

<pre><code class="language-shell"> interface eth0      
 mcast_src_ip 192.168.137.153
 priority 101  
 state BACKUP #两台机器都配置为改值</code></pre>

配置完成后重启两台服务器的keeplived

<pre><code class="language-shell">systemctl restart keepalived</code></pre>

验证：

关闭master，查看ip，可以发现虚拟IP已经飘逸到另外一台服务器了，重启启动keeplived后又回来了。

说明部署成功

最后，登录数据库授权虚拟ip地址段可以连接mysql后就行了虚拟ip的地址

任意一台机器测试登录，可以正常登录便实现了高可用