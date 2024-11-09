---
title: K8s部署kubernetes-dashboard
author: Kubehan
type: post
date: 2020-04-01T06:07:59+08:00
url: /2106.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/04/040120_0607_K8skubernet1.png
views:
  - 1258
  - 1258
classic-editor-remember:
  - classic-editor
  - classic-editor
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/04/k8s-e1585734577105.jpg
  - https://www.kubehan.cn/wp-content/uploads/2020/04/k8s-e1585734577105.jpg
zm_like:
  - 1
  - 1
baidu_record:
  - 1
post_style:
  - sidebar
cao_vip_rate:
  - 1
categories:
  - K8s日志管理可视化
  - K8s集群监控
  - Kubernetes

---
本人在百度上面搜索了超级多的yaml文件来部署dashboard始终未成功，部署文件不生效就是文件不存在，很是头疼，这里将最近2019年11月成功部署的记录进行笔记存下来，仅供参考

<pre><code class="language-bash">cd /dashboard/
wget https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml</code></pre>

更改yaml文件

<pre><code class="language-bash">sed -i \&#039;s/k8s.gcr.io/loveone/g\&#039; kubernetes-dashboard.yaml

sed -i \&#039;/targetPort:/a\\ \\ \\ \\ \\ \\ nodePort: 30001\\n\\ \\ type: NodePort\&#039; kubernetes-dashboard.yaml</code></pre>

创建 

<pre><code class="language-bash">kubectl create -f kubernetes-dashboard.yaml</code></pre>

查看部署情况

<pre><code class="language-bash">kubectl get deployment kubernetes-dashboard -n kube-system
kubectl get pods -n kube-system -o wide
kubectl get services -n kube-system
netstat -ntlp|grep 30001</code></pre>

访问https://ip:30001

获取令牌 

<pre><code class="language-bash">kubectl get secret -n kube-system</code></pre>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/04/040120_0607_K8skubernet1.png" alt="" /> 

<pre><code class="language-bash">kubectl describe -n kube-system secret {NAME}</code></pre>

这里千万不要直接粘贴复制过去。因为你粘贴的会带有一个空格，必须先粘贴到文本里面把空格删除了才行<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/04/040120_0607_K8skubernet2.png" alt="" /> 成功了