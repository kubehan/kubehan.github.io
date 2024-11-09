---
title: Linux mysql8的安装与主从复制的配置
author: Kubehan
type: post
date: 2020-03-24T05:14:00+08:00
url: /1523.html
Baidusubmit:
  - 1
  - 1
views:
  - 1218
  - 1218
categories:
  - Linux运维

---
<p style="text-align: justify">
  <h2>
    服务器准备<br />
  </h2>
</p>

准备服务器Server1和Server2，如果在同一个服务器的话则安装mysql时需要改变其端口。 

<p style="text-align: justify">
  <h2>
    卸载mysql<br />
  </h2>
</p>

在安装之前必须先检查主机上有没有安装过mysql，如果安装过的话必须先卸载。 

<p style="text-align: justify">
  <h2>
    安装mysql<br />
  </h2>
</p>

下载软件包： 

wget https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm 

本地安装： 

yum localinstall mysql80-community-release-el7-1.noarch.rpm 

安装mysql： 

yum install mysql-community-server 

设为开机启动： 

systemctl enable mysqld 

systemctl daemon-reload 

启动mysql： 

systemctl start mysqld 

以上步骤就安装好mysql8了。 

<p style="text-align: justify">
  <h3>
    获取mysql的临时密码：<br />
  </h3>
</p>

grep 'temporary password' /var/log/mysqld.log 

登录mysql： 

mysql -uroot -p 

会提示输入密码，输入之前获取的临时密码即可登录。 

此时需要修改mysql的密码，要不然之后的步骤也会强制提示你需要修改密码： 

ALTER USER 'root'@'localhost' IDENTIFIED BY '121b33dAj934J1^Sj9ag'; 

mysql8默认对密码的强度有要求，需要设置复杂一点，要不然也会提示错误。 

<p style="text-align: justify">
  <h3>
    刷新配置：<br />
  </h3>
</p>

FLUSH PRIVILEGES; 

<p style="text-align: justify">
  <h1>
    主从配置<br />
  </h1>
</p>

在主从配置之前需要确保两台mysql需要同步的库状态一致。 

<p style="text-align: justify">
  <h2>
    主<br />
  </h2>
</p>

配置文件默认在/etc/my.cnf下。 

在配置文件中新增配置： 

<span style="background-color:yellow">[mysqld]<br /> </span>

<span style="background-color:yellow">## 同一局域网内注意要唯一<br /> </span>

<span style="background-color:yellow">server-id=100<br /> </span>

<span style="background-color:yellow">## 开启二进制日志功能，可以随便取（关键）<br /> </span>

<span style="background-color:yellow">log-bin=mysql-bin<br /> </span>

<span style="background-color:yellow">修改配置后需要重启才能生效：<br /> </span>

<span style="background-color:yellow">service mysql restart<br /> </span>

<span style="background-color:yellow">重启之后进入mysql：</span> 

mysql -uroot -p 

<p style="text-align: justify">
  <h3>
    授权<br />
  </h3>
</p>

在master数据库创建数据同步用户，授予用户 slave REPLICATION SLAVE权限和REPLICATION CLIENT权限，用于在主从库之间同步数据。 

CREATE USER 'slave'@'%' IDENTIFIED BY '密码'; 

GRANT REPLICATION SLAVE, REPLICATION CLIENT ON \*.\* TO 'slave'@'ip地址'; 

语句中的ip地址为%代表所有服务器都可以使用这个用户，如果想指定特定的ip，将%改成ip即可。 

<p style="text-align: justify">
  <h3>
    查看主mysql的状态：<br />
  </h3>
</p>

show master status; 

记录下File和Position的值，并且不进行其他操作以免引起Position的变化。 

<p style="text-align: justify">
  <h2>
    从<br />
  </h2>
</p>

在从my.cnf配置中新增： 

<span style="background-color:yellow">mysqld]<br /> </span>

<span style="background-color:yellow">## 设置server_id,注意要唯一<br /> </span>

<span style="background-color:yellow">server-id=101<br /> </span>

<span style="background-color:yellow">## 开启二进制日志功能，以备Slave作为其它Slave的Master时使用<br /> </span>

<span style="background-color:yellow">log-bin=mysql-slave-bin<br /> </span>

<span style="background-color:yellow">## relay_log配置中继日志<br /> </span>

<span style="background-color:yellow">relay_log=edu-mysql-relay-bin<br /> </span>

<span style="background-color:yellow">修改配置后需要重启才能生效：<br /> </span>

<span style="background-color:yellow">service mysql restart</span> 

重启之后进入mysql： 

<p style="text-align: justify">
  <h3>
    参数设置<br />
  </h3>
</p>

mysql -uroot -p 

change master to master\_host='主库的地址',master\_user='slave', master\_password='@#$Rfg345634523rft4fa', master\_port=3306, master\_log\_file='mysql-bin.000001', master\_log\_pos=2830, master\_connect\_retry=30; 

 

 

 

 

 

 

 

<span style="background-color:yellow">master_host ：Master的地址<br /> </span>

<span style="background-color:yellow">master_port：Master的端口号<br /> </span>

<span style="background-color:yellow">master_user：用于数据同步的用户<br /> </span>

<span style="background-color:yellow">master_password：用于同步的用户的密码<br /> </span>

<span style="background-color:yellow">master_log_file：指定 Slave 从哪个日志文件开始复制数据，即上文中提到的 File 字段的值<br /> </span>

<span style="background-color:yellow">master_log_pos：从哪个 Position 开始读，即上文中提到的 Position 字段的值<br /> </span>

<span style="background-color:yellow">master_connect_retry：如果连接失败，重试的时间间隔，单位是秒，默认是60秒</span> 

<p style="text-align: justify">
  <h3>
    在从mysql中查看主从同步状态：<br />
  </h3>
</p>

show slave status \G; 

此时的SlaveIORunning 和 SlaveSQLRunning 都是No，因为我们还没有开启主从复制过程。 

<p style="text-align: justify">
  <h3>
    开启主从复制：<br />
  </h3>
</p>

start slave; 

<p style="text-align: justify">
  <h3>
    再次查看同步状态：<br />
  </h3>
</p>

show slave status \G; 

SlaveIORunning 和 SlaveSQLRunning 都是Yes说明主从复制已经开启。 

 

<p style="text-align: justify">
  <h2>
    错误排查思路<br />
  </h2>
</p>

若SlaveIORunning一直是Connecting，有下面4种原因： 

1、网络不通，检查ip端口 

2、密码不对，检查用于同步的用户名和密码 

3、pos不对，检查Master的Position 

4、mysql8特有的密码规则问题引起： 

ALTER USER 'slave'@'%' IDENTIFIED WITH mysql\_native\_password BY '@#$Rfg345634523rft4fa'; 

将密码规则修改为：mysql\_native\_password 

如果需要指定想要主从同步哪个数据库，可以在master的my.cnf添加配置： 

binlog-do-db：指定mysql的binlog日志记录哪个db 

或者在slave的my.cnf添加配置： 

replicate-do-db=需要复制的数据库名，如果复制多个数据库，重复设置这个选项即可 replicate-ignore-db=需要复制的数据库名，如果复制多个数据库，重复设置这个选项即可 

如果想要同步所有库和表，在从mysql执行： 

STOP SLAVE SQL_THREAD; 

CHANGE REPLICATION FILTER REPLICATE\_DO\_DB = (); 

start SLAVE SQL_THREAD; 

如果以上步骤出现问题，可以查看日志： 

/etc/log/mysqld.log