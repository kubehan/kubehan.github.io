---
title: 让其他物理主机通过NAT网络访问到Hyper-V的虚拟机
author: Kubehan
type: post
date: 2020-06-28T02:52:10+08:00
url: /2633.html
Baidusubmit:
  - 1
views:
  - 1045
zm_like:
  - 1
categories:
  - Linux运维

---
让其他物理主机通过NAT网络访问到Hyper-V的虚拟机

有时候我们会需要在其他的物理主机访问到Hyper-V虚拟主机，但是虚拟机主机使用的网络是宿主主机NAT模式，两者处于不同的网络中，这时候要让其他  
物理机访问到虚拟机可以使用端口映射功能，下面例子仅供参考，本人已亲测成功

下面是hyper-v共享IP端口映射一些常用命令  
共享IP端口映射一些常用命令  
一、查询端口映射情况

<pre><code class="language-bash">netsh interface portproxy show v4tov4</code></pre>

查询这个IP所有的端口映射。  
netsh interface portproxy show v4tov4|find "192.168.1.123"  
二、增加一个端口映射

<pre><code class="language-bash">netsh interface portproxy add v4tov4 listenport=140 listenaddress=192.168.1.93 connectaddress=192.168.137.140 connectport=22</code></pre>

例如：

<pre><code class="language-bash">netsh interface portproxy add v4tov4 listenport=8888 listenaddress=118.123.13.180 connectaddress=192.168.1.10 connectport=2222</code></pre>

三、删除一个端口映射

<pre><code class="language-bash">netsh interface portproxy delete v4tov4 listenaddress=主IP listenport=外网端口</code></pre>

到这里你就可以使用1.93这个宿主主机的140端口连接137.140这个主机的22端口了  
记得把宿主主机防火墙关闭了