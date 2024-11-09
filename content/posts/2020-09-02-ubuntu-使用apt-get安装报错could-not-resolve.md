---
title: Ubuntu 使用apt-get安装报错Could not resolve
author: Kubehan
type: post
date: 2020-09-02T07:16:11+08:00
url: /2799.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/09/1599030961-6174220490ceaba.png
views:
  - 5731
post_style:
  - sidebar
cao_vip_rate:
  - 1
categories:
  - Linux运维

---
报错信息如下

<pre><code class="language-bash">root@k8s-1:/etc/apt/apt.conf.d# apt-get update
Err:1 http://mirrors.aliyun.com/ubuntu bionic InRelease
  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
Err:2 http://mirrors.aliyun.com/ubuntu bionic-security InRelease
  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
Err:3 http://mirrors.aliyun.com/ubuntu bionic-updates InRelease
  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
Err:4 http://mirrors.aliyun.com/ubuntu bionic-proposed InRelease
  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
Err:5 http://mirrors.aliyun.com/ubuntu bionic-backports InRelease
  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
Reading package lists... Done
W: Failed to fetch http://mirrors.aliyun.com/ubuntu/dists/bionic/InRelease  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
W: Failed to fetch http://mirrors.aliyun.com/ubuntu/dists/bionic-security/InRelease  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
W: Failed to fetch http://mirrors.aliyun.com/ubuntu/dists/bionic-updates/InRelease  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
W: Failed to fetch http://mirrors.aliyun.com/ubuntu/dists/bionic-proposed/InRelease  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
W: Failed to fetch http://mirrors.aliyun.com/ubuntu/dists/bionic-backports/InRelease  Could not resolve &#039;mirrors.tuna.tsinghua.cn&#039;
W: Some index files failed to download. They have been ignored, or old ones used instead.
</code></pre>

这里总结比较常见的几种方法  
1.报错大意为不能解析到某个地址  
可能的情况有地址不可用，也就是你源配置有误，百度镜像源找阿里，清华，网易等源来配置  
2.源正确仍然无法解析？  
dns配置有误  
配置dns

<pre><code class="language-bash">vim /etc/systemd/resolved.conf

[Resolve]
DNS=8.8.8.8
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
#DNSStubListener=yes</code></pre>

检查自己的dns配置  
3.前面2个都没问题。还不能apt  
检查是否设置了apt代理，我就是设置了代理，而代理不能用，导致无法使用，那么你需要把代理地址改成有效地址，我是直接取消代理，  
代理地址配置文件：

<pre><code class="language-bash">root@k8s-1:/etc/apt/apt.conf.d# pwd
/etc/apt/apt.conf.d
root@k8s-1:/etc/apt/apt.conf.d# vim 90curtin-aptproxy

Acquire::http::Proxy "https://mtsinghua.cn/ubuntu";
Acquire::https::Proxy "https://a.tsinghua.cn/ubuntu";
</code></pre>

改成有效地址或直接删除、再apt-get update

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1599030961-6174220490ceaba.png" alt="file" />