---
title: Kubernetes Etcd 数据备份与恢复
author: Kubehan
type: post
date: 2020-08-27T05:57:14+08:00
url: /2767.html
cao_vip_rate:
  - 1
post_style:
  - sidebar
views:
  - 1996
categories:
  - K8s日志管理可视化

---
系统环境：

Etcd 版本：3.4.3  
Kubernetes 版本：1.17.4  
Kubernetes 安装方式：Kubeadm  
一、简介Kubernetes 使用 Etcd 数据库实时存储集群中的数据，可以说 Etcd 是 Kubernetes 的核心组件，犹如人类的大脑。如果 Etcd 数据损坏将导致 Kubernetes 不可用，在生产环境中 Etcd 数据是一定要做好高可用与数据备份，这里介绍下如何备份与恢复 Etcd 数据。

## 二、备份 Etcd 数据 {#wow1}

**1、查询 ETCD 镜像**

查询当前 Kubernetes 使用的 Etcd 使用的镜像，记住镜像名称与版本：

    $ docker images | grep etcd</p>
    <p>k8s.gcr.io/etcd                              3.4.3-0   303ce5db0e90    5 months ago    288MB
    registry.aliyuncs.com/google_containers/etcd 3.4.3-0   303ce5db0e90    5 months ago    288MB
    

> 由于 k8s.gcr.io 镜像仓库国内被墙，所以使用的是阿里云的 etcd 镜像

**2、备份 Etcd 数据**

> 本人采用的是 Kubeadm 安装的 Kubernetes 集群，采用镜像方式部署的 Etcd，所以操作 Etcd 需要使用 Etcd 镜像提供的 Etcdctl 工具。如果你是非镜像方式部署 Etcd，可以直接使用 Etcdctl 命令备份数据。

运行 Etcd 镜像，并且使用镜像内部的 etcdctl 工具连接 etcd 集群，执行数据快照备份：

  * /bin/sh -c：执行 shell 命令
  * --env：设置环境变量，指定 etcdctl 工具使用的 API 版本
  * -v：docker 挂载选项，用于挂载 Etcd 证书相关目录以及备份数据存放的目录
  * --cacert：etcd CA 证书
  * --key：etcd 客户端证书 key
  * --cert：etcd 客户端证书 crt
  * --endpoints：指定 ETCD 连接地址
  * etcdctl snapshot save：etcd 数据备份

    <code>$ docker run --rm                                    \<br>-v /data/backup:/backup                              \<br>-v /etc/kubernetes/pki/etcd:/etc/kubernetes/pki/etcd \<br>--env ETCDCTL_API=3                                  \<br>k8s.gcr.io/etcd:3.4.3-0                              \<br>/bin/sh -c "etcdctl --endpoints=<a href="https://192.168.11.105:2379">https://192.168.11.105:2379</a> \<br>--cacert=/etc/kubernetes/pki/etcd/ca.crt                  \<br>--key=/etc/kubernetes/pki/etcd/healthcheck-client.key     \<br>--cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt    \<br>snapshot save /backup/etcd-snapshot.db"</code>

## 三、恢复 ETCD 数据 {#wow2}

在 Etcd 数据损坏时，可以通过 Etcd 备份数据进行数据恢复，先暂停 Kubernetes 相关组件，然后进入 Etcd 镜像使用 etcdctl 工具执行恢复操作。

**1、暂停 Kube-Apiserver 与 Etcd 镜像**

在恢复 Etcd 数据前，需要停止&nbsp;`kube-apiserver`&nbsp;与&nbsp;`etcd`&nbsp;镜像，因为当这俩镜像停止后 Kubernetes 会自动重启这俩镜像，所以我们可以先暂时移除&nbsp;`/etc/kubernetes/manifests`&nbsp;目录，Kubernetes 检测这个目录文件不存在时会停止 Kubernetes 系统相关镜像，使其不能重启，方便我们进行后续的操作。

    ## 移除且备份 /etc/kubernetes/manifests 目录
    $ mv /etc/kubernetes/manifests /etc/kubernetes/manifests.bak</p>
    <p>查看 kube-apiserver、etcd 镜像是否停止
    $ docker ps|grep etcd && docker ps|grep kube-apiserver</p>
    <h2>备份现有 Etcd 数据</h2>
    <p>$ mv /var/lib/etcd /var/lib/etcd.bak
    

**2、恢复 Etcd 数据**

运行 Etcd 镜像，然后执行数据恢复，默认会恢复到&nbsp;`/default.etcd/member/`&nbsp;目录下，这里使用&nbsp;`mv`&nbsp;命令在移动到挂载目录&nbsp;`/var/lib/etcd/`&nbsp;下。

  * /bin/sh -c：执行 shell 命令
  * --env：设置环境变量，指定 etcdctl 工具使用的 API 版本
  * -v：docker 挂载选项，用于挂载 Etcd 证书相关目录以及备份数据存放的目录
  * etcdctl snapshot restore：etcd 数据恢复。

    $ docker run --rm              \
    -v /data/backup:/backup        \
    -v /var/lib/etcd:/var/lib/etcd \
    --env ETCDCTL_API=3            \
    k8s.gcr.io/etcd:3.4.3-0        \
    /bin/sh -c "etcdctl snapshot restore /backup/etcd-snapshot.db; mv /default.etcd/member/ /var/lib/etcd/"
    

**3、恢复 Kube-Apiserver 与 Etcd 镜像**

将&nbsp;`/etc/kubernetes/manifests`&nbsp;目录恢复，使 Kubernetes 重启&nbsp;`Kube-Apiserver`&nbsp;与&nbsp;`Etcd`&nbsp;镜像：

    $ mv /etc/kubernetes/manifests.bak /etc/kubernetes/manifests
    

**4、执行 Kubectl 命令进行检测**

执行 Kubectl 命令进行检测，查看命令是否能够正常执行：

    $ kubectl get node