---
title: 思科asa5520防火墙配置Virtual Private Network
author: Kubehan
type: post
date: 2020-03-02T05:43:21+08:00
url: /445.html
wb_dl_type:
  - 0
  - 0
views:
  - 867
  - 867
wb_dl_mode:
  - 0
  - 0
wb_down_local_url:
  - 
  - 
wb_down_url_ct:
  - 
  - 
wb_down_url:
  - 
  - 
wb_down_pwd:
  - 
  - 
post_views_count:
  - 0
  - 0
categories:
  - Linux运维

---
<!-- wp:heading -->

## 一、实验拓扑

<!-- /wp:heading -->

  
<!-- wp:image {"width":632,"height":334} --><figure class="wp-block-image is-resized"></figure> 

<!-- /wp:image -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading --></p> 

## 二、\***是什么？

<!-- /wp:heading -->

  
<!-- wp:paragraph -->

×××（Virtual Private Network，虚拟专用网）就是在两个网络实体之间建立的一种受保护的连接，这两个实体可以通过点到点的链路直接相连，也可以通过 Internet 相连。

<!-- /wp:paragraph -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading --></p> 

## 三、实验目的

<!-- /wp:heading -->

  
<!-- wp:paragraph -->

1.使用\***让Client1可以访问Server1的web  
2.查看管理连接sa的状态，并清除sa记录  
3.设置pat，让pc机访问外网200.0.0.2，并抓包查看

<!-- /wp:paragraph -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading --></p> 

## 四、配置设置

<!-- /wp:heading -->

  
<!-- wp:paragraph -->

设备ip设置  
Pc1机ip设置：10.2.2.1 掩码255.255.255.0 网关：10.2.2.254  
Client1的ip设置：10.1.1.1 掩码255.255.255.0 网关：10.1.1.254  
Server1的ip设置：192.168.1.0 掩码255.255.255.0 网关：192.168.1.254  
——————————————————————————————————  
防火墙FW1网关设置  
G0：网关10.1.1.254 掩码：255.255.255.0  
G1：网关200.0.0.1 掩码：255.255.255.0  
G2：网关10.2.2.254 掩码：255.255.255.0  
——————————————————————————————————  
防火墙Fw2网关设置  
G0：网关192.168.1.254 掩码：255.255.255.0  
G1：网关200.0.0.2 掩码：255.255.255.0

<!-- /wp:paragraph -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading --></p> 

## 五、防火墙配置思路及代码展示

<!-- /wp:heading -->

  
<!-- wp:heading {"level":3} -->

### 1.防火墙1（asa1）

<!-- /wp:heading -->

  
<!-- wp:paragraph -->

a、进入端口g0设置区域为inside1，安全级别100，设置ip网关10.1.1.254 。进入端口g1设置区域为outside，安全级别0，设置ip为200.0.0.1。进入端口g2设置区域为 inside2，安全级别100，设置ip网关10.2.2.254。  
b、设置静态路由：route outside 0.0.0.0 0.0.0.0 200.0.0.2  
c、设置acl：允许10.1.1.0网段访问192.168.1.0网段  
d、配置ISAKMP策略、置加密协议 isakmp 、配置IPSec策略（转换集）、配置加密映射集、将映射集应用在接口

<!-- /wp:paragraph -->

  
<!-- wp:code -->

<pre class="wp-block-code"><code>ciscoasa(config)# hostname asa1 //修改名称为asa1

asa1(config)# int g0 //进入端口g0
asa1(config-if)# no shutdown //开启端口
asa1(config-if)#nameif inside1 //设置区域名称inside1
asa1(config-if)#security-level 100  //设置区域安全级别100
asa1(config-if)#ip address 10.1.1.254 255.255.255.0 //设置网关ip

asa1(config)# int g1 //进入端口g1
asa1(config-if)# no shutdown //开启端口
asa1(config-if)#nameif outside //设置区域名称outside
asa1(config-if)#security-level 0 //设置区域安全级别0
asa1(config-if)#ip address 200.0.0.1 255.255.255.0 //设置网关ip

asa1(config)# int g2 //进入端口g2
asa1(config-if)# no shutdown //开启端口
asa1(config-if)#nameif inside2 //设置区域名称inside2
asa1(config-if)#security-level 100 //设置区域安全级别100
asa1(config-if)#ip address 10.2.2.254 255.255.255.0  //设置网关ip
asa1(config-if)#exit //退出

设置静态路由
asa1(config-if)# route outside 0.0.0.0 0.0.0.0 200.0.0.2  //设置静态路由，使用默认路由吓一跳为200.0.0.2

配置ISAKMP策略
asa1(config)# crypto ikev1 enable outside //开启加密协议isakmp，名称ikev1，方向outside区域
asa1(config)# crypto ikev1 policy 1 //设置加密协议isakmp策略为1
asa1(config-ikev1-policy)# encryption aes  //使用aes加密
asa1(config-ikev1-policy)# hash sha   //设置哈希算法为sha
asa1(config-ikev1-policy)# authentication pre-share  //采用预共享密钥方式
asa1(config-ikev1-policy)# group 2 //指定DH算法的密钥长度为组2
asa1(config-ikev1-policy)# exit //退出

设置加密协议 isakmp 
asa1(config)# tunnel-group 200.0.0.2 type ipsec-l2l //设置隧道目标地址为200.0.0.2，类型为点对点方式
asa1(config)# tunnel-group 200.0.0.2 ipsec-attributes  //设置隧道的加密策略
asa1(config-tunnel-ipsec)# ikev1 pre-shared-key tedu //设置加密策略的密钥为tedu
asa1(config-tunnel-ipsec)# exit //退出

配置acl
asa1(config)#access-list 100 permit ip 10.1.1.0 255.255.255.0 192.168.1.0 255.255.255.0 //设置acl允许10.1.1.0网段访问192.168.1.0网段

配置IPSec策略（转换集）
asa1(config)# crypto ipsec ikev1 transform-set yf-set esp-aes esp-sha-hmac  //配置IPSec策略（转换集）

配置加密映射集
asa1(config)# crypto map yf-map 1 match address 100 //设置加密图 yf-map匹配acl 100
asa1(config)# crypto map yf-map 1 set peer 200.0.0.2  //建立加密图邻居为200.0.0.2
asa1(config)# crypto map yf-map 1 set ikev1 transform-set yf-set  //设置加密图IPSec策略转换集

asa1(config)# crypto map yf-map interface outside //将映射集应用在接口</code></pre>

<!-- /wp:code -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading {"level":3} --></p> 

### 2.防火墙2（asa2）

<!-- /wp:heading -->

  
<!-- wp:paragraph -->

a、进入端口g0设置区域为inside，安全级别100，设置ip网关192.168.1.254 。进入端口g1设置区域为outside，安全级别0，设置ip为200.0.0.2。  
b、设置静态路由：route outside 0.0.0.0 0.0.0.0 200.0.0.1  
c、设置acl：允许192.168.1.0网段访问10.1.1.0网段  
d、配置ISAKMP策略、置加密协议 isakmp 、配置IPSec策略（转换集）、配置加密映射集、将映射集应用在接口

<!-- /wp:paragraph -->

  
<!-- wp:code -->

<pre class="wp-block-code"><code>ciscoasa(config)# hostname asa2 //修改名称为asa2

Asa2(config)# int g0 //进入端口g0
Asa2(config-if)# no shutdown //开启端口
Asa2(config-if)#nameif inside //设置区域名称inside
Asa2(config-if)#security-level 100 //设置区域安全级别100
Asa2(config-if)#ip address 192.168.1.254 255.255.255.0 //设置网关ip

Asa2(config)# int g1 //进入端口g1
Asa2(config-if)# no shutdown //开启端口
Asa2(config-if)#nameif outside //设置区域名称outside
Asa2(config-if)#security-level 0 //设置区域安全级别0
Asa2(config-if)#ip address 200.0.0.2 255.255.255.0 //设置网关ip

设置静态路由
asa2(config)# route outside 0.0.0.0 0.0.0.0 200.0.0.1 //设置静态路由，使用默认路由吓一跳为200.0.0.1

配置ISAKMP策略
asa2(config)# crypto ikev1 enable outside  //开启加密协议isakmp，名称ikev1，方向outside区域（名称与防火墙一要一致）
asa2(config)# crypto ikev1 policy 1 //设置加密协议isakmp策略为1（策略号也要一致）
asa2(config-ikev1-policy)# encryption aes //使用aes加密
asa2(config-ikev1-policy)# hash sha //设置哈希算法为sha
asa2(config-ikev1-policy)# authentication pre-share  //采用预共享密钥方式
asa2(config-ikev1-policy)# group 2 //指定DH算法的密钥长度为组2
asa2(config-ikev1-policy)# exit //退出

设置加密协议 isakmp 
asa2(config)# tunnel-group 200.0.0.1 type ipsec-l2l  //设置隧道目标地址为200.0.0.1，类型为点对点方式
asa2(config)# tunnel-group 200.0.0.1 ipsec-attributes //设置隧道的加密策略
asa2(config-tunnel-ipsec)# ikev1 pre-shared-key tedu //设置加密策略的密钥为tedu（双向密钥应唯一）
asa2(config-tunnel-ipsec)# exit //退出

设置acl
asa2(config)#access-list 100 extended permit ip 192.168.1.0 255.255.255.0 10.1.1.0 255.255.255.0  //设置acl允许192.168.1.0网段访问10.1.1.0网段

配置IPSec策略（转换集）
asa2(config)# crypto ipsec ikev1 transform-set yf-set esp-aes esp-sha-hmac  //配置IPSec策略（转换集）

配置加密映射集 
asa2(config)# crypto map yf-map 1 match address 100 //设置加密图 yf-map匹配acl 100
asa2(config)# crypto map yf-map 1 set peer 200.0.0.1  //建立加密图邻居为200.0.0.1
asa2(config)# crypto map yf-map 1 set ikev1 transform-set yf-set  //设置加密图IPSec策略转换集

asa2(config)# crypto map yf-map interface outside //将映射集应用在接口</code></pre>

<!-- /wp:code -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading {"level":3} --></p> 

### 测试：Client1访问Server1的web

<!-- /wp:heading -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:paragraph -->

在Server1上设置web

Client1访问Server1的web

<!-- /wp:paragraph -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading {"level":3} --></p> 

### 测试：查看管理连接sa的状态

<!-- /wp:heading -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:paragraph -->

`asa2(config)# show crypto isakmp sa //查看管理连接sa的状态`

`Asa1(config)# show crypto isakmp sa //查看管理连接sa的状态`

清除sa记录

<!-- /wp:paragraph -->

  
<!-- wp:code -->

<pre class="wp-block-code"><code>asa1(config)# clear crypto isakmp sa //清除sa记录
asa2(config)# clear crypto isakmp sa //清除sa记录</code></pre>

<!-- /wp:code -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading {"level":3} --></p> 

### 测试：设置pat，让pc机访问外网200.0.0.2（对物理接口进行抓包）

<!-- /wp:heading -->

  
<!-- wp:code -->

<pre class="wp-block-code"><code>asa1(config)# object network ob-inside2  ////设置对象网络名称为ob-inside2
asa1(config-network-object)# subnet 10.2.2.0 255.255.255.0 //设置要转换的内网ip段
asa1(config-network-object)# nat (inside2,outside) dynamic interface //设置动态pat，让内网（inside2）ip动态的转换为出口端ip
asa1(config-network-object)# exit  //退出</code></pre>

<!-- /wp:code -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:image --><figure class="wp-block-image"></figure> 

<!-- /wp:image -->

  
<!-- wp:paragraph -->

在物理机上设置ip

使用抓包工具wireshark抓包  
点击在点击进入  
抓包结果

查看转换条目  
`asa1(config)# show xlate //查看转换条目`

<!-- /wp:paragraph -->

  
<!-- wp:separator -->

<hr class="wp-block-separator" />

<!-- /wp:separator -->

  
<!-- wp:heading --></p> 

## 六、拓扑图解释

<!-- /wp:heading -->

  
<!-- wp:paragraph -->

有些人看不懂华为的云是做什么用的，我就再此解释一下，首先我使用的防火墙是在vmware虚拟机上启用的，因此我需要使用物理端口映射到云上，让华为的其他设备可以进行访问。  
如果还有什么不懂或者疑问可以直接留言询问我！

<!-- /wp:paragraph -->