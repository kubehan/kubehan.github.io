---
title: cisco asa l2tp over ipsec配置详解
author: Kubehan
type: post
date: 2020-03-02T05:24:59+08:00
excerpt: cisco asa l2tp over ipsec配置详解
url: /442.html
wb_dl_type:
  - 0
  - 0
views:
  - 954
  - 954
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
zm_favorites:
  - 1
  - 1
categories:
  - Linux运维

---
<!-- wp:paragraph -->

1 创建VPN地址池

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp;ciscoasa(config)# ip local pool vpnpool 192.168.151.11-192.168.151.15 mask 255.255.255.0

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

2 配置Ipsec&nbsp;加密算法为3DES和SHA

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; ciscoasa(config)# crypto ipsec transform-set TRANS\_ESP\_3DES_SHA esp-3des esp-sha-hmac

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

3 配置IPSec传输模式为transport，默认为tunnel 模式(L2TP 只支持transport)

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; Ciscoasa（config）#crypto ipsec transform-set TRANS\_ESP\_3DES_SHA mode transport

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

4 使用传输组定义动态加密策略

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; Ciscoasa(config)# crypto dynamic-map outside\_dyn\_map 10 set transform-set TRANS\_ESP\_3DES_SHA

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

5 定义加密映射并应用到外网接口上(outside)

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; #crypto map outside\_map 10 ipsec-isakmp dynamic outside\_dyn_map

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

# crypto map outside_map interface outside

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp;6 外网接口上开启isakmp策略支持

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

Ciscoasa(config) crypto isakmp enable outside

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp;7 &nbsp;定义isakmp 策略

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;Ciscoasa(config)# crypto &nbsp;isakmp &nbsp; policy &nbsp; 10

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-isakmp-policy)# authentication &nbsp;pre-share

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-isakmp-policy)# encryption &nbsp;3des&nbsp;

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-isakmp-policy)# hash sha

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-isakmp-policy)# group &nbsp;2

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-isakmp-policy)# lifetime 86400

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-isakmp-policy)# exit

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; 8 设置nat 穿越

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config)# crypto &nbsp;isakmp &nbsp; nat-traversal &nbsp;10

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; 9 配置默认内部组策略

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config)# group-policy &nbsp; DefaultRAGroup &nbsp;internal

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; 10 配置默认内部组策略属性

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config)# group-policy DefaultRAGroup attributes

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-group-policy)# vpn-tunnel-protocol &nbsp; IPSec &nbsp; l2tp-ipsec

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-group-policy)# default-domain &nbsp;value &nbsp;cisco.com

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-group-policy)# dns-server &nbsp;value 202.96.209.133

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

注：配置L2TP over IPsec为vpn隧道的协议，必须要加IPSec，只有l2tp-ipsec，vpn是拨不通的

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; 11创建本地用户，及为用户配置密码，并指明加密算法

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config)# username &nbsp;frank password &nbsp; frank &nbsp;mschap&nbsp;

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

  12 创建默认的隧道组，一定要使用defaultRAGroup，L2TP不支持其他组，并定义认证方式为本地。

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config)# tunnel-group &nbsp;DefaultRAGroup general-attributes

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-tunnel-general)# authentication-server-group &nbsp; LOCAL

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-tunnel-general)# default-group-policy &nbsp;DefaultRAGroup

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-tunnel-general)#address-pool vpnpool

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-tunnel-general)#exit

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

13 制定用户所在的组策略

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-tunnel-general)#username frank attributes

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-username)# vpn-group-policy DefaultRAGroup

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-username)# vpn-tunnel-protocol &nbsp;IPSec l2tp-ipsec

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

ciscoasa(config-username)# exit

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

14 &nbsp;配置默认隧道组的ipsec属性,并配置默认隧道组认证方式为ms-chap-v2

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config)# tunnel-group &nbsp; DefaultRAGroup &nbsp;ppp-attributes

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-ppp)# &nbsp;authentication &nbsp; ms-chap-v2

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;ciscoasa(config-ppp)#exit

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

15 &nbsp;客户端设置

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp;Win 7 需要修改注册表

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

&nbsp; &nbsp; &nbsp; [HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\PolicyAgent]

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

"AssumeUDPEncapsulationContextOnSendRule"=dword:00000002

<!-- /wp:paragraph -->

  
<!-- wp:paragraph -->

16 在客户端上创建连接到工作区域VPN 连接，并设置vpn 的属性&nbsp;

<!-- /wp:paragraph -->