---
title: Kubernetes主机和容器的监控方案
author: Kubehan
type: post
date: 2020-03-27T09:55:48+08:00
url: /2019.html
views:
  - 804
  - 804
thumbnail:
  - https://www.kubernetes.org.cn/img/2017/07/20170728092406.jpg
  - https://www.kubernetes.org.cn/img/2017/07/20170728092406.jpg
classic-editor-remember:
  - classic-editor
  - classic-editor
categories:
  - K8s基础概念
  - K8s常用对象
  - K8s日志管理可视化
  - K8s部署
  - K8s集群监控
  - Kubernetes

---
> **摘要**：随着Docker容器云的广泛应用，大量的业务软件运行在容器中，这使得对docker容器的监控越来越重要。传统的监控系统大多数是针对物理机或者虚拟机设计的，而容器的特点不同与传统的物理机或者虚拟机，如果还是采用传统的监控系统，则会增加监控复杂程度，那么如何对容器进行监控呢？

大家晚上好，今天很高兴能在这里和大家一起交流和分享在工作中的一些经验和总结。都知道监控在运维体系乃至产品的整个生命中期都是重要的一个环节，针对不同的应用场景，监控方案也会有很大的不同。本次就和大家分享一下我在开发我们公司新产品ufleet的监控模块时的一些技术总结，如果有错误的地方，欢迎大家指出。主要内容有:

1.数据的采集方式

2.监控原理

3.容器的监控方案

4.kubernetes上的主机和容器的监控

5.监控工具的对比

一个完整的监控体系包括：采集数据、分析存储数据、展示数据、告警以及自动化处理、监控工具自身的安全机制，接下来会对数据的采集和监控原理深入讲解，其他部分会在一些架构中穿插讲解。

## 一 、数据的采集方式

1.命令行方式。比如在linux系统上使用top，vmstat，netstat写一些shell脚本进行数据的采集，再把数据存储在文本文件中进行处理。

2.嵌入式。通过在进程中运行agent的方式获取应用的状态。如目前的APM产品都是通过将监控工具嵌入到应用内部进行数据采集。

3.主动输出。提前在应用中埋点，应用主动上报。比如一些应用系统的业务状态，可以通过在日志中主动输出状态用于采集。

4.旁路式。通过外部获取的方式采集数据。比如对网站url的探测，模拟业务的报文 ，对服务器的ping，流量的监控。可以通过在交换机上将流量进行端口复制，将源始流量复制到另一个端口后再进行处理，这样这业务系统是完全没有侵入。

5.远程接入。通过对应用进程接口调用获取应用的状态。比如使用JMX的方式连接到java进程中，对进程的状态进行采集。

6.入侵式。不同于嵌入式，入侵式的agent是独立运行的进程，而不是运行在进程中。这个目前监控工具比较常用的方式，比如zabbix，在主机上运行一个进程进行相关数据的采集。

## 二、监控原理

具体监控指标总结如下：

  * 首先是容器本身资源使用情况：cpu，内存，网络，磁盘
  * 物理机的资源使用情况：cpu，内存，网络，磁盘
  * 物理机上容器镜像情况，名字，大小，版本。

### 1.主机的监控

**（1）Cpu数据**

使用top命令可以查看当前cpu使用情况，源文件来自/proc/stat

采样两个足够短的时间间隔的Cpu快照，分别记作t1,t2，其中t1、t2的结构均为：

(user、nice、system、idle、iowait、irq、softirq、stealstolen、guest)的9元组;

a) 计算总的Cpu时间片totalCpuTime

  * 把第一次的所有cpu使用情况求和，得到s1;
  * 把第二次的所有cpu使用情况求和，得到s2;
  * s2 – s1得到这个时间间隔内的所有时间片，即totalCpuTime = j2 – j1 ;

b) 计算空闲时间idle

  * idle对应第四列的数据，用第二次的第四列- 第一次的第四列即可
  * idle=第二次的第四列- 第一次的第四列

c) 计算cpu使用率

  * pcpu =100* (total-idle)/total

**（2）linux内存监控**

使用free命令可以查看当前内存使用情况。

其数据来源是来自/proc/meminfo文件

常用的计算公式：

> real\_used = used\_mem – buffer – cache
> 
> real\_free = free\_mem + buffer + cache
> 
> total\_mem = used\_mem + free_mem

**（3） Network数据**

/proc/net/dev保存着有关网络的数据

如计算一段时间sec秒内的网络平均流量：

> infirst=$(awk ‘/’$eth’/{print $1 }’ /proc/net/dev |sed ‘s/’$eth’://’)
> 
> outfirst=$(awk ‘/’$eth’/{print $10 }’ /proc/net/dev)
> 
> sumfirst=$(($infirst+$outfirst))
> 
> sleep $sec”s”
> 
> inend=$(awk ‘/’$eth’/{print $1 }’ /proc/net/dev |sed ‘s/’$eth’://’)
> 
> outend=$(awk ‘/’$eth’/{print $10 }’ /proc/net/dev)
> 
> sumend=$(($inend+$outend))
> 
> sum=$(($sumend-$sumfirst))
> 
> aver=$(($sum/$sec))

### 2.docker的监控

docker自身提供了一种内存监控的方式，即可以通过docker stats对容器内存进行监控。

该方式实际是通过对cgroup中相关数据进行取值从而计算得到。其数据来源是/sys/fs/cgroup

docker client相关代码入口可参考：/docker/docker/api/client/stats.go#141

docker daemon相关代码入口可参考：/docker/docker/daemon/daemon.go#1474

**（1）Cpu数据**

docker daemon会记录这次读取/sys/fs/cgroup/cpuacct/docker/  [containerId]/cpuacct.usage的值，作为cpu\_total\_usage；并记录了上一次读取的该值为      pre\_cpu\_total\_usage；读取/proc/stat中cpu field value，并进行累加，得到system\_usage;并    记录上一次的值为pre\_system\_usage；读取/sys/fs/cgroup/cpuacct/docker/       [containerId]/cpuacct.usage\_percpu中的记录，组成数组per\_cpu\_usage\_array；

docker stats计算Cpu Percent的算法：

cpu\_delta = cpu\_total\_usage – pre\_cpu\_total\_usage;

system\_delta = system\_usage – pre\_system\_usage;

CPU % = ((cpu\_delta / system\_delta) \* length(per\_cpu\_usage_array) ) \* 100.0

**(2) Memory数据**

读取/sys/fs/cgroup/memory/docker/[containerId]/memory.usage\_in\_bytes的值，作为    mem\_usage；如果容器限制了内存，则读取/sys/fs/cgroup/memory/docker/  [id]/memory.limit\_in\_bytes作为mem\_limit，否则mem\_limit = machine\_mem；docker stats计算 Memory数据的算法：

MEM USAGE = mem_usage

MEM LIMIT = mem_limit

MEM % = (mem\_usage / mem\_limit) * 100.0

**（3）Network Stats数据：**

获取属于该容器network namespace veth pairs在主机中对应的veth*虚拟网卡EthInterface       数组，然后循环数组中每个网卡设备，读取/sys/class/net/[device]/statistics/rx\_bytes得到rx\_bytes, 读取/sys/class/net/[device]/statistics/tx\_bytes得到对应的tx\_bytes。

将所有这些虚拟网卡对应的rx\_bytes累加得到该容器的rx\_bytes。

将所有这些虚拟网卡对应的tx\_bytes累加得到该容器的tx\_bytes。

docker stats计算Network IO数据的算法：

NET I = rx_bytes

NET O = tx_bytes

## 三、容器的监控方案

### 1.单台主机容器监控：

（1）docker stats

单台主机上容器的监控实现最简单的方法就是使用命令Docker stats，就可以显示所有容器的资源使用情况.

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2433" src="https://www.kubernetes.org.cn/img/2017/07/20170728092313.jpg" sizes="(max-width: 839px) 100vw, 839px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092313.jpg 839w, https://www.kubernetes.org.cn/img/2017/07/20170728092313-300x112.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092313-768x287.jpg 768w" alt="" width="839" height="314" /> 

这样就可以查看每个容器的CPU利用率、内存的使用量以及可用内存总量。请注意，如果你没有限制容器内存，那么该命令将显示您的主机的内存总量。但它并不意味着你的每个容器都能访问那么多的内存。另外，还可以看到容器通过网络发送和接收的数据总量

虽然可以很直观地看到每个容器的资源使用情况，但是显示的只是一个当前值，并不能看到变化趋势。

（2）Google的 cAdvisor 是另一个知名的开源容器监控工具:

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2434" src="https://www.kubernetes.org.cn/img/2017/07/20170728092323.jpg" sizes="(max-width: 843px) 100vw, 843px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092323.jpg 843w, https://www.kubernetes.org.cn/img/2017/07/20170728092323-300x154.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092323-768x394.jpg 768w" alt="" width="843" height="433" /> 

只需在宿主机上部署cAdvisor容器，用户就可通过Web界面或REST服务访问当前节点和容器的性能数据(CPU、内存、网络、磁盘、文件系统等等)，非常详细。

它的运行方式也有多种：

a.直接下载命令运行

下载地址：https://github.com/google/cadvisor/releases/latest

格式:  nohup /root/cadvisor  -port=10000 &>>/var/log/kubernetes/cadvisor.log &

访问： http://ip:10000/

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2435" src="https://www.kubernetes.org.cn/img/2017/07/20170728092333.jpg" sizes="(max-width: 725px) 100vw, 725px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092333.jpg 725w, https://www.kubernetes.org.cn/img/2017/07/20170728092333-300x132.jpg 300w" alt="" width="725" height="318" /> 

b.以容器方式运行

<pre>docker pull index.alauda.cn/googlelib/cadvisor</pre>

运行：

<pre>docker run -d --volume=/:/rootfs:ro --volume=/var/run:/var/run:rw –volume=/sys:/sys:ro       --volume=/var/lib/docker/:/var/lib/docker:ro --publish=8080:8080  --name=cadvisor                      

index.alauda.cn/googlelib/cadvisor:latest</pre>

c.kubelet选项：

在启动kubelete时候，启动cadvisor

cAdvisor当前都是只支持http接口方式，被监控的容器应用必须提供http接口，所以能力较弱。在Kubernetes的新版本中已经集成了cAdvisor，所以在Kubernetes架构下，不需要单独再去安装cAdvisor，可以直接使用节点的IP加默认端口4194就可以直接访问cAdvisor的监控面板。UI界面如下：

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2436" src="https://www.kubernetes.org.cn/img/2017/07/20170728092348.jpg" sizes="(max-width: 843px) 100vw, 843px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092348.jpg 843w, https://www.kubernetes.org.cn/img/2017/07/20170728092348-300x151.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092348-768x385.jpg 768w" alt="" width="843" height="423" /> 

因为cAdvisor默认是将数据缓存在内存中，在显示界面上只能显示1分钟左右的趋势，所以历史的数据还是不能看到，但它也提供不同的持久化存储后端，比如influxdb等，同时也可以根据业务的需求，只利用cAdvisor提供的api接口，定时去获取数据存储到数据库中，然后定制自己的界面。

如需要通过cAdvisor查看某台主机上某个容器的性能数据只需要调用：       http://<host\_ip>:4194/v1.3/subcontainers/docker/<container\_id>

cAdvisor的api接口返回的数据结构如下：

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2437" src="https://www.kubernetes.org.cn/img/2017/07/20170728092357.jpg" sizes="(max-width: 838px) 100vw, 838px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092357.jpg 838w, https://www.kubernetes.org.cn/img/2017/07/20170728092357-300x162.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092357-768x415.jpg 768w" alt="" width="838" height="453" /> 

可以根据这些数据分别计算出 CPU、内存、网络等资源的使用或者占用情况。

## 四、kubernetes上的监控

### 1.容器的监控

在Kubernetes监控生态中，一般是如下的搭配使用：

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2438" src="https://www.kubernetes.org.cn/img/2017/07/20170728092406.jpg" sizes="(max-width: 810px) 100vw, 810px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092406.jpg 810w, https://www.kubernetes.org.cn/img/2017/07/20170728092406-300x161.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092406-768x412.jpg 768w" alt="" width="810" height="435" /> 

**（1）Cadvisor+InfluxDB+Grafana：**

Cadvisor：将数据，写入InfluxDB

InfluxDB ：时序数据库，提供数据的存储，存储在指定的目录下

Grafana ：提供了WEB控制台，自定义查询指标，从InfluxDB查询数据，并展示

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2439" src="https://www.kubernetes.org.cn/img/2017/07/20170728092415.jpg" sizes="(max-width: 730px) 100vw, 730px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092415.jpg 730w, https://www.kubernetes.org.cn/img/2017/07/20170728092415-300x171.jpg 300w" alt="" width="730" height="415" /> 

cAdivsor虽然能采集到监控数据，也有很好的界面展示，但是并不能显示跨主机的监控数据，当主机多的情况，需要有一种集中式的管理方法将数据进行汇总展示，最经典的方案就是 cAdvisor+ Influxdb+grafana，可以在每台主机上运行一个cAdvisor容器负责数据采集，再将采集后的数据都存到时序型数据库influxdb中，再通过图形展示工具grafana定制展示面板。

在上面的安装步骤中，先是启动influxdb容器，然后进行到容器内部配置一个数据库给cadvisor专用，然后再启动cadvisor容器，容器启动的时候指定把数据存储到influxdb中，最后启动grafana容器，在展示页面里配置grafana的数据源为influxdb，再定制要展示的数据，一个简单的跨多主机的监控系统就构建成功了。

**（2）Kubernetes——Heapster+InfluxDB+Grafana：**

Heapster：在k8s集群中获取metrics和事件数据，写入InfluxDB，heapster收集的数据比cadvisor多，却全，而且存储在influxdb的也少。

InfluxDB：时序数据库，提供数据的存储，存储在指定的目录下。

Grafana：提供了WEB控制台，自定义查询指标，从InfluxDB查询数据，并展示。

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2440" src="https://www.kubernetes.org.cn/img/2017/07/20170728092422.jpg" sizes="(max-width: 812px) 100vw, 812px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092422.jpg 812w, https://www.kubernetes.org.cn/img/2017/07/20170728092422-300x72.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092422-768x185.jpg 768w" alt="" width="812" height="196" /> 

Heapster是一个收集者，将每个Node上的cAdvisor的数据进行汇总，然后导到InfluxDB。Heapster的前提是使用cAdvisor采集每个node上主机和容器资源的使用情况，再将所有node上的数据进行聚合，这样不仅可以看到Kubernetes集群的资源情况，还可以分别查看每个node/namespace及每个node/namespace下pod的资源情况。这样就可以从cluster，node，pod的各个层面提供详细的资源使用情况。

### 2、kubernetes中主机监控方案：

prometheus

prometheus是个集 db、graph、statistic、alert 于一体的监控工具，安装也非常简单，下载包后做些参数的配置，比如监控的对象就可以运行了，默认通过9090端口访问。

**（1） 部署node-exporter容器**

node-exporter 要在集群的每台主机上部署，使用主机网络，端口是9100 如果有多个K8S集群，则要在多个集群上部署，部署node-exporter的命令如下：

\# kubectl create -f node-exporter-deamonset.yaml

获取metrics数据http://ip:9100/metrics

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2441" src="https://www.kubernetes.org.cn/img/2017/07/20170728092431.jpg" sizes="(max-width: 845px) 100vw, 845px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092431.jpg 845w, https://www.kubernetes.org.cn/img/2017/07/20170728092431-300x161.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092431-768x413.jpg 768w" alt="" width="845" height="454" /> 

返回的数据结构不是json格式，如果要使用该接口返回的数据，可以通过正则匹配，匹配出需要的数据，然后在保存到数据库中。

**（2） 部署Prometheus和Grafana**

Prometheus 通过配置文件发现新的节点，文件路径是/sd/*.json,可以通过修改已有的配置文件，添加新的节点纳入监控，命令如下：

\# kubectl create -f prometheus-file-sd.yaml

**（3）查看Prometheus监控的节点**

Prometheus 的访问地址是：http://192.168.xxx.xxx:31330

通过网页查看监控的节点Status –> Targets

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2442" src="https://www.kubernetes.org.cn/img/2017/07/20170728092440.jpg" sizes="(max-width: 665px) 100vw, 665px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092440.jpg 665w, https://www.kubernetes.org.cn/img/2017/07/20170728092440-300x138.jpg 300w" alt="" width="665" height="307" /> 

<img loading="lazy" decoding="async" class="alignnone size-full wp-image-2443" src="https://www.kubernetes.org.cn/img/2017/07/20170728092448.jpg" sizes="(max-width: 841px) 100vw, 841px" srcset="https://www.kubernetes.org.cn/img/2017/07/20170728092448.jpg 841w, https://www.kubernetes.org.cn/img/2017/07/20170728092448-300x106.jpg 300w, https://www.kubernetes.org.cn/img/2017/07/20170728092448-768x270.jpg 768w" alt="" width="841" height="296" /> 

**（4）另外可以配置Grafana展示Prometheus输出的监控数据，配置仪表盘等。**

Grafana 访问地址是:http://192.168.xxx.xxx:31331

账号:admin 密码：admin

注：系统预置了几个常用监控仪表盘配置，更多的配置可以到官方网站下载

## 五、监控工具的对比

以上从几个典型的架构上介绍了一些监控，但都不是最优实践。需要根据生产环境的特点结合每个监控产品的优势来达到监控的目的。比如Grafana的图表展示能力强，但是没有告警的功能，那么可以结合Prometheus在数据处理能力改善数据分析的展示。下面列了一些监控产品，但并不是严格按表格进行分类，比如Prometheus和Zabbix都有采集，展示，告警的功能。都可以了解一下，各取所长。

<table>
  <tr>
    <td width="57">
      采集
    </td>
    
    <td width="586">
      cAdvisor, Heapster, collectd, Statsd, Tcollector, Scout
    </td>
  </tr>
  
  <tr>
    <td width="57">
      存储
    </td>
    
    <td width="586">
      InfluxDb, OpenTSDB, Elasticsearch
    </td>
  </tr>
  
  <tr>
    <td width="57">
      展示
    </td>
    
    <td width="586">
      Graphite, Grafana, facette, Cacti, Ganglia, DataDog
    </td>
  </tr>
  
  <tr>
    <td width="57">
      告警
    </td>
    
    <td width="586">
      Nagios, prometheus, Icinga, Zabbix
    </td>
  </tr>
</table>

本文是摘自kubernetes中文社区：https://www.kubernetes.org.cn/2432.html [url  href=[链接地址][1]]kubernetes中文社区[/url]

 [1]: https://www.kubernetes.org.cn/2432.html