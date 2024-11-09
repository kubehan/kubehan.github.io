---
title: kubernetes-dashboard不支持谷歌浏览器webui及显示英文解决办法
author: Kubehan
type: post
date: 2020-04-13T05:11:42+08:00
url: /2240.html
Baidusubmit:
  - 1
  - 1
views:
  - 2386
  - 2386
zm_like:
  - 7
  - 7
post_style:
  - sidebar
cao_vip_rate:
  - 1
categories:
  - K8s部署
  - K8s集群监控
  - Kubernetes
  - Linux运维

---
K8S Dashboard安装好以后，通过Firefox浏览器解决办法kubernetes-dashboard是可以打开的，但通过Google Chrome浏览器，无法成功浏览页面，对于一些新手来说，傻乎乎的用谷歌打开，缺怎么也打不开，一睹站在怀疑人生的道路上。这里分别给出两盒问题的解决方案  
**不支持谷歌浏览器webui解决办法：**  
自己创建证书，不使用kubeadmz自动生成的证书

<pre><code class="language-bash">mkdir key && cd key
openssl genrsa -out dashboard.key 2048</code></pre>

# 10.10.10.139为master节点的IP地址

<pre><code class="language-bash">openssl genrsa -out dashboard.key 2048 
openssl req -new -out dashboard.csr -key dashboard.key -subj &#039;/CN=10.10.10.139&#039;
openssl x509 -req -in dashboard.csr -signkey dashboard.key -out dashboard.crt </code></pre>

# 删除原有证书

# 注意新版的Dashboard的namespace已经变为kubernetes-dashboard了，旧版本的在默认的namespace

<pre><code class="language-bash">kubectl delete secret kubernetes-dashboard-certs -n kubernetes-dashboard</code></pre>

# 创建新证书的secret

<pre><code class="language-bash">kubectl create secret generic kubernetes-dashboard-certs --from-file=dashboard.key --from-file=dashboard.crt -n kubernetes-dashboard
</code></pre>

# 查找正在运行的pod

<pre><code class="language-bash">kubectl get pod -n kubernetes-dashboard
</code></pre>

# 删除pod，这里补充一下，可能有些小伙不敢删除这个pod，放心好了，删除后，新的pod会自动启动起来。

<pre><code class="language-bash">kubectl delete po pod的name -n kubernetes-dashboard</code></pre>

# 如果pod比较多的时候，可以使用以下这条命令批量删除。

<pre><code class="language-bash">kubectl get pod -n kubernetes-dashboard | grep -v NAME | awk \&#039;{print kubectl delete po  $1  -n kubernetes-dashboard}\&#039; | sh</code></pre>

这时，再次刷新Chrome浏览器的Dashboard页面就能正常浏览了  
**显示英文解决办法**  
因为官方默认采用zh显示中文，而不是zh-CN。而浏览器默认为zh-CN。只需要改一下显示语言即可。后期可能会自适应中文。找到谷歌浏览器的设置页\---\----->语言\---\---\--->将中文置顶，原来是中文简体刷新页面变能正常显示中文了