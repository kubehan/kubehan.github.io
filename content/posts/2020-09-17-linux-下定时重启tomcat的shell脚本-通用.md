---
title: Linux 下定时重启Tomcat的shell脚本--通用
author: Kubehan
type: post
date: 2020-09-17T05:45:08+08:00
url: /2888.html
post_style:
  - sidebar
cao_price:
  - 10
cao_vip_rate:
  - 1
cao_downurl:
  - https://www.kubehan.cn/wp-content/uploads/2020/05/公众号.jpg
cao_diy_btn:
  - 大佬打赏一个吗
views:
  - 1716
categories:
  - SHELL
  - Tomcat

---
# Linux 下定时重启Tomcat的shell脚本--通用

脚本使用说明：  
如果没有把JAVA_HOME放到环境变量的话需要取消注释

<pre><code class="language-actionscript">#export JAVA_HOME=/usr/local/jdk1.7.0_79
#export JRE_HOME=$JAVA_HOME/jre</code></pre>

不然直接修改tomcat所在路径即可  
脚本执行原理：  
优先使用shutdown命令关闭Tomcat  
用shutdown命令关闭失败，准备kill进程

<pre><code class="language-bash">#!/bin/bash
#auto bakcup mysql db
#by authprs hanwei  2019/12/03
. /etc/profile

#export JAVA_HOME=/usr/local/jdk1.7.0_79
#export JRE_HOME=$JAVA_HOME/jre

tomcatPath="//root/apache-tomcat-9.0.27"
binPath="$tomcatPath/bin"
echo "[info][$(date +'%F %H:%M:%S')]正在监控tomcat，路径：$tomcatPath"
pid=`ps -ef|grep java | grep catalina | awk '{print $2}'`
if [ -n "$pid" ]; then
    echo "[info][$(date +'%F %H:%M:%S')]正在运行的tomcat进程为：$pid"
    echo "[info][$(date +'%F %H:%M:%S')]tomcat已经启动，准备使用shutdown命令关闭..."
    $binPath"/shutdown.sh"
    sleep 2
    pid=`ps -ef|grep java | grep catalina | awk '{print $2}'`
    if [ -n "$pid" ]; then
        echo "[info][$(date +'%F %H:%M:%S')]使用shutdown命令关闭失败，准备kill进程..."
        kill -9 $pid
        echo "[info][$(date +'%F %H:%M:%S')]kill进程完毕！"
        sleep 1
    else
        echo "[info][$(date +'%F %H:%M:%S')]使用shutdown命令关闭成功！"
    fi
else
    echo "[info][$(date +'%F %H:%M:%S')]tomcat未启动！"
fi
echo "[info][$(date +'%F %H:%M:%S')]准备启动tomcat..."
$binPath"/startup.sh"
</code></pre>