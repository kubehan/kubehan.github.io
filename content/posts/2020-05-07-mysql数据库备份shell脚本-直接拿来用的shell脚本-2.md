---
title: mysql数据库备份shell脚本—直接拿来用的shell脚本
author: Kubehan
type: post
date: 2020-05-07T11:19:12+08:00
url: /2499.html
Baidusubmit:
  - 1
views:
  - 1158
zm_like:
  - 1
categories:
  - Linux运维

---
mysql数据库备份shell脚本—直接拿来用的shell脚本  
经过我的一次次的尝试，脚本优化后写出来这个脚本给大家使用  
使用中遇到问题的可以留言联系方式解答

<pre><code class="language-bash">#!/bin/bash
#auto bakcup mysql db
#by authprs hanwei  2019/12/03
#describe:备份多个数据库只需要在对应的目录建一个只含有数据库库名的mysql.list再执行脚本就行了
########################   定义&变量   ###################################
date=`date +"%Y-%m-%d"`
BAK_DIR=/data/mysql_backup/   #目录
#MYSQLDB=&#039;cat ./mysql.list&#039;     #数据库名
MYSQLUSR=root   #用户名
MYSQPASSWD=123456       #密码
MYSQLCMD=/usr/local/mysql/bin/mysqldump     #调用的数据库命令 
########################  开始  ###############################
#判断用户是否为超级用户。
if [ $UID -ne 0 ];then
    echo "请使用超级用户。"
    exit
fi

#判断MySQL数据库的备份目录目录是否存在。
if [ ! -d $BAK_DIR ];then
    mkdir -p $BAK_DIR
    echo -e "33[32m $BAK_DIR 目录创建成功！33[0m"
else
    echo -e "33[32m $BAK_DIR 已存在!33[0m"
fi

#备份MySQL数据库的命令。
#命令格式为 -u用户 -p密码 -d指定的数据库
for database in `cat ${BAK_DIR}/mysql.list`
do
    $MYSQLCMD -u${MYSQLUSR} -p${MYSQPASSWD} -x ${database} &gt; $BAK_DIR/${date}.${database}.sql
    #检查是否备份成功。
    if [ $? -eq 0 ];then
        echo -e "33[32m 备份$database 成功！33[0m"
    else
        echo -e "33[32m 备份$database 失败，请检查...33[0m"
        rm -rf $BAK_DIR/${date}.${database}.sql
    fi
done</code></pre>