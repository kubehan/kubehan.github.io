---
title: 部署 metrics-server 后无法使用 top 命令
author: Kubehan
type: post
date: 2020-12-31T07:38:11+08:00
url: /3136.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/12/1609400212-9f25cf3407bb7ad.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 1568
categories:
  - Kubernetes

---
等待metrics-server的pod完全启动后，使用 kubectl top 命令，报错如下：  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/1609400161-01c41bdf20214be.png" alt="file" /> 

kubectl top 报错  
通过 kubectl logs 查看 pod 中应用日志报错如下：  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/1609400167-2a78d376becf54d.png" alt="file" /> 

metrics-server 应用报错  
根据报错信息初步看是 Pod 中 DNS 无法解析出节点 k8s-master 、k8s-node1、k8s-node2 的 IP，修改其 yaml 文件，让 Pod 通过 IP 直接访问，而不是通过主机名

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/1609400173-6493a36fe575c6c.png" alt="file" />  
使用IP通信  
修改 yaml 后，测试仍然异常：  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/1609400184-c850d2dc95c97e9.png" alt="file" /> 

再次修改 yaml 文件，忽略证书校验步骤，已确保 https 调用成功  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/1609400202-513833e7fcc9e6d.png" alt="file" /> 

添加 kubelet-insecure-tls  
再次测试，可正常使用 kubectl top

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/1609400212-9f25cf3407bb7ad.png" alt="file" />  
【总结】  
从 github 上拉下来的 yaml 需要先修改3个部分再进行应用，镜像源 kubelet通信方式 以及 证书校验配置【以下贴出部分yaml作为参考】。

<pre><code class="language-bash"> containers:
      - name: metrics-serve
        image: lizhenliang/metrics-server:v0.3.7
        imagePullPolicy: IfNotPresent
        args:
          - --cert-dir=/tmp
          - --secure-port=4443
          - --kubelet-preferred-address-types=InternalIP
          - --kubelet-insecure-tls</code></pre>