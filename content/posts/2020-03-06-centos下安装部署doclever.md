---
title: Centos下安装部署DOCLever
author: Kubehan
type: post
date: 2020-03-06T08:13:57+08:00
excerpt: |
  DOClever是一个可视化接口管理工具 ,可以分析接口结构，校验接口正确性， 围绕接口定义文档，通过一系列自动化工具提升我们的协作效率。
  
      DOClever前后端全部采用了javascript来作为我们的开发语言，前端用的是vue+element UI，后端是express+mongodb，这样的框架集成了高并发，迭代快的特点，保证系统的稳定可靠。
url: /716.html
views:
  - 1039
  - 1039
dwqr_like:
  - 1
  - 1
post_views_count:
  - 0
  - 0
zm_favorites:
  - 1
  - 1
zm_like:
  - 2
Baidusubmit:
  - 1
categories:
  - Linux运维
tags:
  - DOCLever

---
DOClever是一个可视化接口管理工具 ,可以分析接口结构，校验接口正确性， 围绕接口定义文档，通过一系列自动化工具提升我们的协作效率。  
DOClever前后端全部采用了javascript来作为我们的开发语言，前端用的是vue+element UI，后端是express+mongodb，这样的框架集成了高并发，迭代快的特点，保证系统的稳定可靠。

<pre><code class="language-bash">#!/bin/bash
# @Author: HanWei
# @Date:   2020-03-06 14:57:43
# @Last Modified by:   HanWei
# @Last Modified time: 2020-05-22 11:32:43
# @E-mail: han_wei_95@163.com
# 
# 
# 教程描述：安装DOCLever,需要提前准备安装包
HOME=/www/software
DOC_HOME=/www/server
function download ()
{
    cd /www/package
    wget https://nodejs.org/dist/v10.13.0/node-v10.13.0-linux-x64.tar.xz
    wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-4.0.4.tgz
}

function install_node ()
{
    cd /www/package
    tar xvJf node-v10.13.0-linux-x64.tar.xz
    mv node-v10.13.0-linux-x64 ${HOME}/node
    cat &gt;&gt; /ete/profile &lt;&lt; EOF
    export NODE_HOME=${HOME}/node  
    export PATH=$NODE_HOME/bin:$PATH
EOF
source /etc/profile
node -v
}

function install_mongodb ()
{
    cd /www/package
    tar zxvf mongodb-linux-x86_64-4.0.4.tgz
    mv mongodb-linux-x86_64-4.0.4 ${HOME}/mongodb
    cd ${HOME}/mongodb
    mkdir db 
    mkdir logs 
    cd bin
    cat &gt;&gt;  mongodb.conf &lt;&lt; EOF
port=27017
dbpath=/usr/local/mongodb/db
logappend=true
fork=true
logpath=/usr/local/mongodb/logs/mongpdb.log
#nohttpinterface=true
EOF
./mongod -f mongodb.conf
chmod +x /etc/rc.d/rc.local
echo "${HOME}/mongodb/bin/mongod --config ${HOME}/mongodb/bin/mongodb.conf" &gt;&gt; /etc/rc.d/rc.local
}

function install_DOC ()
{
    echo "安装前将源码解压到${DOC_HOME}/DOClever/"
    node ${DOC_HOME}/DOClever/Server/bin/www
    npm install -g cnpm --registry=https://registry.npm.taobao.org
    cnpm install forever -g
    forever start ${DOC_HOME}/DOClever/Server/bin/www
    echo "/www/software/node/bin/forever start ${DOC_HOME}/DOClever/Server/bin/www 2&gt;&1 &gt;&gt;/tmp/doclever.log  & " &gt;&gt; /etc/rc.d/rc.local
    echo "这里 /www/DOClever/config.json 可以更改端口号"
}
download
install_node
install_mongodb
install_DOC
echo "安装完成"</code></pre>