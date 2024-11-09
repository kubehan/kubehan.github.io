---
title: 'K8S部署elasticsearch集群报错  {"type":"master_not_discovered_exception","reason":null}'
author: Kubehan
type: post
date: 2022-02-20T14:38:21+08:00
url: /3243.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2022/02/1645367863-9333c2235376e9b.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 2162
categories:
  - Linux运维

---
报错现象：

<pre><code class="language-bash">curl -X GET 10.254.16.193:9200/_cat/indices
{"error":{"root_cause":[{"type":"master_not_discovered_exception","reason":null}],"type":"master_not_discovered_exception","reason":null},"status":503}r</code></pre>

日志现象：

<pre><code class="language-bash">{"type": "server", "timestamp": "2022-02-20T14:24:45,555Z", "level": "WARN", "component": "o.e.c.c.ClusterFormationFailureHelper", "cluster.name": "docker-clustde.name": "es-0", "message": "master not discovered yet, this node has not previously joined a bootstrapped (v7+) cluster, and this node must discover master-elodes [es-0.es-svc, es-1.es-svc, es-2.es-svc] to bootstrap a cluster: have discovered [{es-0}{hdINfvU5Sw2ILBX7y40ITw}{n75Kx9lARoa_j7HsIcnUmg}{10.254.16.226}{10.26:9300}{dilm}{ml.machine_memory=2147483648, xpack.installed=true, ml.max_open_jobs=20}, {es-1}{zZ1MoUB8QDKGtw-o9liqxw}{InbT9AiiTUuhahQwrgDCCw}{10.254.63.104}{10104:9300}{dilm}{ml.machine_memory=2147483648, ml.max_open_jobs=20, xpack.installed=true}, {es-2}{oqiTb3GyTpSruJRzffGsng}{KzDbF_i3T_iR2XpmFIilyA}{10.254.34.153}{4.153:9300}{dilm}{ml.machine_memory=2147483648, ml.max_open_jobs=20, xpack.installed=true}]; discovery will continue using [10.254.63.104:9300, 10.254.34.153:93 hosts providers and [{es-0}{hdINfvU5Sw2ILBX7y40ITw}{n75Kx9lARoa_j7HsIcnUmg}{10.254.16.226}{10.254.16.226:9300}{dilm}{ml.machine_memory=2147483648, xpack.instal, ml.max_open_jobs=20}] from last-known cluster state; node term 0, last-accepted version 0 in term 0" }</code></pre>

es配置文件：  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2022/02/1645367790-6f1f390cb106653.png" alt="file" />  
报错原因：  
ELK集群配置中，node.master: true表示该节点有作为master节点的资格。一个ELK集群，可以有多个节点拥有该资格，然后通过自举的方式确定哪个节点是master节点。

上述报错是因为在ELK集群中，无法通过自举的方式在有资格成为master节点的节点中，自举出master节点，导致集群无法发现master节点  
解决方案：  
添加下述配置：  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2022/02/1645367863-9333c2235376e9b.png" alt="file" /> 

特别注意： value是节点name 而不是节点的IP或值SVC name