---
title: Kubernetes (k8s) v1.19.0 安装部署（Kubeadm）
author: Kubehan
type: post
date: 2020-09-07T01:40:29+08:00
url: /2836.html
post_style:
  - sidebar
cao_price:
  - 10
cao_vip_rate:
  - 1
cao_status:
  - 1
cao_downurl:
  - https://www.kubehan.cn/wp-content/uploads/2020/09/1599442795-7ccbee7076c55d7.pdf
cao_diy_btn:
  - Kubernetes (k8s) v1.19.0 安装部署（Kubeadm）.pdf
cao_paynum:
  - 1343
views:
  - 4297
categories:
  - K8s部署
  - Linux运维

---
# Kubernetes (k8s) v1.19.0 安装部署（Kubeadm）

<pre><code class="language-shell">#!/bin/bash
# @Author: HanWei
# @Date:   2020-09-01 15:04:09
# @Last Modified by:   HanWei
# @Last Modified time: 2020-09-01 17:05:38
# @E-mail: han_wei_95@163.comKubernetes (k8s) v1.19.0 安装部署（Kubeadm）</code></pre>

Kubernetes (k8s) v1.19.0 安装部署（Kubeadm）

部署方式：Kubeadm

环境：

<pre><code class="language-shell">k8s-1 2c4G     Ubuntu-18.05     192.168.1.150
k8s-2 2c4G     Ubuntu-18.05     192.168.1.151
k8s-3 2c4G     Ubuntu-18.05     192.168.1.152</code></pre>

k8s环境准备  
系统环境准备  
1、添加解析（全部节点）

<pre><code class="language-shell">cat&gt;&gt;/etc/hosts&lt;&lt;EOF
192.168.1.150 k8s-1
192.168.1.151 k8s-2
192.168.1.152 k8s-3
EOF</code></pre>

2、防火墙及swap相关参数设置

<pre><code class="language-shell"># Turn off and disable the firewalld.
关闭防火墙
ufw disable
# Disable the swap.
swapoff -a
sed -i &#039;s/^.*swap/#&/g&#039; /etc/fstab</code></pre>

## 运行环境准备

1、安装Docker （全部节点）

<pre><code class="language-shell">#!/bin/bash
# I安装docker。设置阿里云的yum源安装新版docker
sudo cp /etc/apt/sources.list /etc/apt/sources_init.list
curl -fsSL https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add -
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

cat &gt; /etc/apt/sources.list &lt;&lt;EOF
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
EOF</code></pre>

<pre><code class="language-shell">#安装docker
# step 1: 安装必要的一些系统工具
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce

# 安装指定版本的Docker-CE:
# Step 1: 查找Docker-CE的版本:
# apt-cache madison docker-ce
#   docker-ce | 17.03.1~ce-0~ubuntu-xenial | https://mirrors.aliyun.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
#   docker-ce | 17.03.0~ce-0~ubuntu-xenial | https://mirrors.aliyun.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
# Step 2: 安装指定版本的Docker-CE: (VERSION例如上面的17.03.1~ce-0~ubuntu-xenial)
# sudo apt-get -y install docker-ce=[VERSION]</code></pre>

<pre><code class="language-shell">#此处配置阿里云为镜像加速，方便国内拉取镜像，并配置cgroupdriver=systemd
cat &gt;&gt; /etc/docker/daemon.json&lt;&lt;EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "registry-mirrors": ["https://xk4sok19.mirror.aliyuncs.com"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

systemctl enable docker
systemctl restart docker
#关注一下Cgroup Driver是否为 systemd及阿里云镜像加速是否成功配置
docker info
root@k8s-1:/etc/apt/apt.conf.d# docker info 
Client:
 Debug Mode: false

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 19.03.12
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: systemd
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  apparmor
  seccomp
   Profile: default
 Kernel Version: 4.15.0-29-generic
 Operating System: Ubuntu 18.04.1 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.936GiB
 Name: k8s-1
 ID: 3VTO:4OVJ:53YS:FUB6:4LC7:I5IF:GBVK:QLSI:IX73:7R2D:LJNB:6QL4
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://xk4sok19.mirror.aliyuncs.com/
 Live Restore Enabled: false

WARNING: No swap limit support
root@k8s-1:/etc/apt/apt.conf.d# </code></pre>

2、安装 kubelet-1.19.0-0 kubeadm-1.19.0-0 kubectl-1.19.0-0 （全部节点）

<pre><code class="language-shell">国内镜像地址
#首先导入 gpg key：
curl -fsSL https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add -
新建 /etc/apt/sources.list.d/kubernetes.list，内容如下
deb https://mirrors.tuna.tsinghua.edu.cn/kubernetes/apt kubernetes-xenial main
也可以不用新建，直接添加到/etc/apt/sources.list
apt-get update更新源
#安装 kubelet-1.19.0-0 kubeadm-1.19.0-0 kubectl-1.19.0-0
apt install  -y kubelet kubeadm kubectl
systemctl restart kubelet ; systemctl enable kubelet</code></pre>

[reply]  
3、安装master（只在master节点执行）  
在安装过程中，可以通过—image-repository来指定镜像仓库：  
初始化方式一：

<pre><code class="language-shell">kubeadm init --image-repository registry.aliyuncs.com/google_containers --kubernetes-version=v1.19.0 --pod-network-cidr=10.244.0.0/16 </code></pre>

初始化方式二：生成初始化配置文件，通过配置文件完成初始化

<pre><code class="language-shell">#配置文件生成：
kubeadm config print init-defaults kubeadm.conf
编辑kubeadm.conf找到serviceSubnet处添加 
podSubnet=10.244.0.0/16
并且修改 
kubernetes-version=v1.19.0
及 
image-repository registry.aliyuncs.com/google_containers
执行初始化：
kubeadm init --config kubeadm.conf --ignore-preflight-errors=all</code></pre>

根据执行的结果操作

要使kubectl适用于您的非root用户，请运行以下命令，这些命令也是kubeadm init输出的一部分：

<pre><code class="language-shell">mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config</code></pre>

注意1：这里用--image-repository选项指定使用阿里云的镜像  
注意2：--pod-network-cidr=10.244.0.0/16 #这里指的是pod的网段  
注意3：如果想安装其他版本的话，直接在--kubernetes-version里指定:

<pre><code class="language-shell">kubeadm init --image-repository registry.aliyuncs.com/google_containers --kubernetes-version=v1.18.2 --pod-network-cidr=10.244.0.0/16</code></pre>

（当然要先安装对应的kubelet的版本）

2、node节点加入集群

<pre><code class="language-shell">kubeadm join --token &lt;token&gt; &lt;control-plane-host&gt;:&lt;control-plane-port&gt; --discovery-token-ca-cert-hash sha256:&lt;hash&gt;</code></pre>

上面的提示中，

<pre><code class="language-shell">kubeadm join 192.168.1.150:6443 --token rzf4r7.y50x0r5zrrkvllao \
--discovery-token-ca-cert-hash sha256:227bf56c53a225da581a414d2a15e6afa3e3b4e1b57553c0eedee843b158f177</code></pre>

是用于node加入到kubernetes集群的命令，如果忘记了保存此命令的话，可以用如下命令获取：  
kubeadm token create --print-join-command  
这个token的24小时有效，过期了需要重新生成

3、安装Pod网络附加组件（在master节点操作就可以）  
wget [<https://docs.projectcalico.org/v3.11/manifests/calico.yaml>][1]  
编辑calico.yaml

# 里面的pod网络默认为192.168.0.0/16

# 找到如下内容，取消注释并将网段改完初始化时候的--pod-network-cidr的值

<pre><code class="language-shell">name: CALICO_IPV4POOL_CIDR
value: "192.168.0.0/16"

kubectl apply -f calico.yaml</code></pre>

再次在master上运行命令 kubectl get nodes查看运行结果：

<pre><code class="language-shell">[root@k8s-master ~]# kubectl get nodes
NAME            STATUS   ROLES    AGE   VERSION
k8s-master      Ready    master   19m    v1.19.0
k8s-node1       Ready    &lt;none&gt;   16m    v1.19.0
k8s-node2       Ready    &lt;none&gt;   16m    v1.19.0</code></pre>

可以看到所有节点的状态已经变为是Ready了。

4、配置自动补全命令

# 安装bash自动补全插件

yum install bash-completion -y

# 设置kubectl与kubeadm命令补全，下次login生效

<pre><code class="language-shell">kubectl completion bash &gt;/etc/bash_completion.d/kubectl
kubeadm completion bash &gt; /etc/bash_completion.d/kubeadm</code></pre>

5、安全配置  
Kubernetes 将Pod调度到Master节点（单机运行K8S）去除 master 的污点  
出于安全考虑，默认配置下Kubernetes不会将Pod调度到Master节点。如果希望将k8s-master也当作Node使用，可以执行如下命令：

<pre><code class="language-shell">kubectl taint node k8s-master node-role.kubernetes.io/master-</code></pre>

其中k8s-master是主机节点hostname如果要恢复Master Only状态，执行如下命令：

<pre><code class="language-shell">kubectl taint node k8s-master node-role.kubernetes.io/master=""</code></pre>

# 这个时间可能清华镜像源还没有k8s1.19的镜像。需要手动下载导入

# 拉取镜像的脚本 在其他的机器上面执行

<pre><code class="language-shell">##!/bin/bash
# Script For Quick Pull K8S Docker Images
# by qiraosky &lt;qiraosky@qq.com&gt;
KUBE_VERSION=v1.19.0
PAUSE_VERSION=3.2
CORE_DNS_VERSION=1.7.0
ETCD_VERSION=3.4.9
#下载master组件镜像
docker pull kubesphere/kube-controller-manager-amd64:$KUBE_VERSION
docker pull kubesphere/kube-apiserver-amd64:$KUBE_VERSION
docker pull kubesphere/kube-scheduler-amd64:$KUBE_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:$ETCD_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:$CORE_DNS_VERSION
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:$PAUSE_VERSION

#下载node组件镜像
docker pull kubesphere/kube-proxy-amd64:$KUBE_VERSION
#docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:$ETCD_VERSION
#docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:$CORE_DNS_VERSION
#docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:$PAUSE_VERSION
#
#镜像打tag
docker tag kubesphere/kube-proxy-amd64:$KUBE_VERSION  k8s.gcr.io/kube-proxy:$KUBE_VERSION
docker tag kubesphere/kube-controller-manager-amd64:$KUBE_VERSION k8s.gcr.io/kube-controller-manager:$KUBE_VERSION
docker tag kubesphere/kube-apiserver-amd64:$KUBE_VERSION k8s.gcr.io/kube-apiserver:$KUBE_VERSION
docker tag kubesphere/kube-scheduler-amd64:$KUBE_VERSION k8s.gcr.io/kube-scheduler:$KUBE_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/pause:$PAUSE_VERSION k8s.gcr.io/pause:$PAUSE_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:$CORE_DNS_VERSION k8s.gcr.io/coredns:$CORE_DNS_VERSION
docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:$ETCD_VERSION k8s.gcr.io/etcd:$ETCD_VERSION

#删除tag
docker rmi kubesphere/kube-proxy-amd64:$KUBE_VERSION
docker rmi kubesphere/kube-controller-manager-amd64:$KUBE_VERSION
docker rmi kubesphere/kube-apiserver-amd64:$KUBE_VERSION
docker rmi kubesphere/kube-scheduler-amd64:$KUBE_VERSION
docker rmi registry.cn-hangzhou.aliyuncs.com/google_containers/pause:$PAUSE_VERSION
docker rmi registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:$CORE_DNS_VERSION
docker rmi registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:$ETCD_VERSION</code></pre>

<pre><code class="language-shell">#!/bin/bash
#镜像导出
KUBE_VERSION=v1.19.0
PAUSE_VERSION=3.2
CORE_DNS_VERSION=1.7.0
ETCD_VERSION=3.4.9
docker save &lt;code>docker images -aq  k8s.gcr.io/kube-proxy:$KUBE_VERSION</code>  &gt; kube-proxy:$KUBE_VERSION.tar.gz
docker save <code>docker images -aq  k8s.gcr.io/kube-controller-manager:$KUBE_VERSION</code>  &gt; kube-controller-manager:$KUBE_VERSION.tar.gz
docker save <code>docker images -aq  k8s.gcr.io/kube-apiserver:$KUBE_VERSION</code>  &gt; kube-apiserver:$KUBE_VERSION.tar.gz
docker save <code>docker images -aq  k8s.gcr.io/kube-scheduler:$KUBE_VERSION</code>  &gt; kube-scheduler:$KUBE_VERSION.tar.gz
docker save <code>docker images -aq  k8s.gcr.io/pause:$PAUSE_VERSION</code>  &gt; pause:$PAUSE_VERSION.tar.gz
docker save <code>docker images -aq  k8s.gcr.io/coredns:$CORE_DNS_VERSION</code>  &gt; coredns:$CORE_DNS_VERSION.tar.gz
docker save <code>docker images -aq  k8s.gcr.io/etcd:$ETCD_VERSION</code>  &gt; etcd:$ETCD_VERSION.tar.gz&lt;/code></pre>

导出的镜像在其他机器上面可以直接导入使用

使用docker load 命令参数导入镜像

示例：docker load -i < 镜像包.tar.gz

<pre><code class="language-shell">root@k8s-1:~# docker load --help
Usage:  docker load [OPTIONS]
Load an image from a tar archive or STDIN
Options:
  -i, --input string   Read from tar archive file, instead of STDIN
  -q, --quiet          Suppress the load output</code></pre>

最后如果有需要pdf版本的教程请右边点击支付1元下载

 [1]: https://docs.projectcalico.org/v3.11/manifests/calico.yaml