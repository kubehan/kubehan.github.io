---
title: FileZilla-Server-FTP的主动模式和被动模式
author: admin
type: post
date: 2020-05-08T04:35:51+08:00
url: /2507.html
Baidusubmit:
  - 1
views:
  - 1285
zm_like:
  - 5
custom_title:
  - FileZilla-Server-FTP的主动模式和被动模式
categories:
  - Linux运维

---
PORT 主动模式：  
用户主机一个随机端口连接FTP SERVER的TCP21端口进行协商；  
用户主机告诉FTP SERVER，我的XXXX端口已经打开，你可以放心大胆的连过来；  
然后FTP SERVER就用TCP20端口连接用户主机的XXXX端口，数据传输开始  
FTP-Client-host 开启一个随机端口 \---\---\---\-----》 FTP Server 的TCP-21 进行协商  
FTP-Client host 把协商好的端口打开 《\---\---- FTP Server 连进来  
FTP Server 用TCP-20 和客户端的随机端口进行数据传输  
要求：  
FTP-Server 的防火墙要开放 TCP 20,21 端口  
FTP-Client 端最好把防火墙关闭，因为用的是随机端口  
一句话，主动模式就是 client 端开放一个随机端口，Server端主动去连接。</p> 

* * *

PASV 被动模式：  
FTP Client 随机端口 \---\---\---\---\----》 FTP Server 的 TCP 21端口进行协商  
FTP Server 告诉FTP Client 我的XXX端口已打开，你可以连过来。  
FTP Client 用一个随机端口去连 FTP Server的 XXX端口，开始数据传输。  
被动模式要求，FTP Server 的防火墙开放 21 和 一个范围内的端口，在被动模式里设置的。  
FTP Client 则要有随机端口可用即可。  
一句话， 被动模式就是 Server 端开放一个随机端口 等待 客户端来连接