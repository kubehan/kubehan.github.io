---
title: 【威胁通告】攻击者利用漏洞攻击Edimax WiFi桥接器
author: Kubehan
type: post
date: 2020-04-27T11:38:31+08:00
url: /2345.html
Baidusubmit:
  - 1
views:
  - 693
zm_like:
  - 1
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/04/frc-c83d4f6e8ba820bb94ca759b7d76e16d.png
categories:
  - 漏洞公告

---
<section style="max-width: 100%;box-sizing: border-box;font-size: 16px;overflow-wrap: break-word !important;"> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  2020年4月14日，Exploit DB公布了一个针对Edimax WiFi桥接器的远程执行漏洞的利用(EDB-ID：48318)，绿盟科技格物实验室结合绿盟威胁情报中心（NTI）对相应设备的暴露情况进行验证，发现2020年年初至今，Edimax WiFi桥接器暴露数量达到6000台以上。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <strong style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;">绿盟威胁情报中心（NTI）已支持对该事件的在线检测（https://nti.nsfocus.com），同时使用绿盟威胁情报赋能的产品也均已支持对该事件的精确检测。</strong>
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;line-height: 1;overflow-wrap: break-word !important;"> <section style="max-width: 100%;box-sizing: border-box;display: inline-block;vertical-align: top;overflow-wrap: break-word !important;">

<span style="max-width: 100%;box-sizing: border-box;width: 0px;display: inline-block;opacity: 0.6;border-left: 0.6em solid rgb(111, 186, 44);overflow-wrap: break-word !important;border-top: 0.5em solid transparent !important;border-bottom: 0.5em solid transparent !important;"></span><span style="max-width: 100%;box-sizing: border-box;width: 0px;display: inline-block;border-left: 0.6em solid rgb(111, 186, 44);overflow-wrap: break-word !important;border-top: 0.5em solid transparent !important;border-bottom: 0.5em solid transparent !important;"></span></section> <section style="padding-left: 3px;max-width: 100%;box-sizing: border-box;display: inline-block;vertical-align: top;line-height: 1.2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  事件脆弱性分析
</p></section> </section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  绿盟科技格物实验室发现除了2020年4月14日由Exploit DB公布的RCE之外，Exploit DB中还存在8个Edimax的漏洞利用。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;line-height: 1;overflow-wrap: break-word !important;"> <section style="max-width: 100%;box-sizing: border-box;display: inline-block;vertical-align: top;overflow-wrap: break-word !important;">

<span style="max-width: 100%;box-sizing: border-box;width: 0px;display: inline-block;opacity: 0.6;border-left: 0.6em solid rgb(111, 186, 44);overflow-wrap: break-word !important;border-top: 0.5em solid transparent !important;border-bottom: 0.5em solid transparent !important;"></span><span style="max-width: 100%;box-sizing: border-box;width: 0px;display: inline-block;border-left: 0.6em solid rgb(111, 186, 44);overflow-wrap: break-word !important;border-top: 0.5em solid transparent !important;border-bottom: 0.5em solid transparent !important;"></span></section> <section style="padding-left: 3px;max-width: 100%;box-sizing: border-box;display: inline-block;vertical-align: top;line-height: 1.2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  事件互联网暴露情况
</p></section> </section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  绿盟科技格物实验室发现2020年年初至今，互联网中暴露的无需认证的Edimax WiFi桥接器共665个，暴露但需要认证的Edimax WiFi桥接器共5580个。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;line-height: 1;overflow-wrap: break-word !important;"> <section style="max-width: 100%;box-sizing: border-box;display: inline-block;vertical-align: top;overflow-wrap: break-word !important;">

<span style="max-width: 100%;box-sizing: border-box;width: 0px;display: inline-block;opacity: 0.6;border-left: 0.6em solid rgb(111, 186, 44);overflow-wrap: break-word !important;border-top: 0.5em solid transparent !important;border-bottom: 0.5em solid transparent !important;"></span><span style="max-width: 100%;box-sizing: border-box;width: 0px;display: inline-block;border-left: 0.6em solid rgb(111, 186, 44);overflow-wrap: break-word !important;border-top: 0.5em solid transparent !important;border-bottom: 0.5em solid transparent !important;"></span></section> <section style="padding-left: 3px;max-width: 100%;box-sizing: border-box;display: inline-block;vertical-align: top;line-height: 1.2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  事件威胁分析
</p></section> </section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  绿盟科技格物实验室发现，在对漏洞利用增加交互的当天（2020年4月18日），便捕获到了利用匿名DNS Log平台对该漏洞的探测行为。次日（2020年4月19日）便捕获到利用该漏洞投递样本的行为。整个事件的流程为：
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  【4月14日】漏洞利用公布。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  【4月18日】紧急在威胁捕获系统中增加了针对该漏洞的交互。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  【4月18日】绿盟科技格物实验室捕获到针对该漏洞的探测行为，值得注意的是，此次捕获到的探测行为不同于往常，其尝试请求dnslog.cn，通过查询dns记录的方式确认设备是否具备脆弱性。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  【4月19日】绿盟科技格物实验室捕获到利用该漏洞投递样本的行为。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  【4月21日】利用该漏洞投递样本的行为出现了爆发的现象。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p></section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  该事件在NTI的查询详情如下：
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;text-align: center;overflow-wrap: break-word !important;"> <section style="max-width: 100%;box-sizing: border-box;vertical-align: middle;display: inline-block;line-height: 0;overflow-wrap: break-word !important;"></section> </section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  涉及到该事件的C&C服务器的详情如下：
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;text-align: center;overflow-wrap: break-word !important;"> <section style="max-width: 100%;box-sizing: border-box;vertical-align: middle;display: inline-block;line-height: 0;overflow-wrap: break-word !important;"></section> </section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  此外，通过绿盟威胁情报中心威胁知识图谱发现，针对Edimax WiFi桥接器漏洞投递的部分样本为Gafgyt家族的变种（如下图所示）。说明部分蠕虫家族已经更新其漏洞库，对这些设备发动攻击。
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;text-align: center;overflow-wrap: break-word !important;"> <section style="max-width: 100%;box-sizing: border-box;vertical-align: middle;display: inline-block;line-height: 0;overflow-wrap: break-word !important;"></section> </section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;text-align: center;overflow-wrap: break-word !important;"> <section style="max-width: 100%;box-sizing: border-box;vertical-align: middle;display: inline-block;line-height: 0;overflow-wrap: break-word !important;"></section> </section> <section powered-by="xiumi.us" style="padding-right: 10px;padding-left: 10px;max-width: 100%;box-sizing: border-box;color: rgb(62, 62, 62);line-height: 2;overflow-wrap: break-word !important;"> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  IOC持续更新中，完整IOC列表可在绿盟威胁情报中心（https://nti.nsfocus.com）获取。
</p>

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
  <br style="max-width: 100%;box-sizing: border-box;overflow-wrap: break-word !important;" />
</p></section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;text-align: center;overflow-wrap: break-word !important;"> <section style="padding-right: 1em;padding-left: 1em;max-width: 100%;box-sizing: border-box;display: inline-block;overflow-wrap: break-word !important;">

<span title="" opera-tn-ra-cell="_$.pages:0.layers:0.comps:17.title1" style="padding: 0.3em 0.5em;max-width: 100%;box-sizing: border-box;display: inline-block;border-radius: 0.5em;background-color: rgb(111, 186, 44);font-size: 14px;color: rgb(255, 255, 255);overflow-wrap: break-word !important;"></p> 

<p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
   绿盟威胁情报中心  
</p>

<p>
  </span></section> <section style="margin-top: -1em;padding: 20px 10px 10px;max-width: 100%;box-sizing: border-box;border-width: 1px;border-style: solid;border-color: rgba(62, 62, 62, 0.17);background-color: rgb(244, 244, 244);overflow-wrap: break-word !important;"> <section powered-by="xiumi.us" style="max-width: 100%;box-sizing: border-box;text-align: justify;font-size: 13px;overflow-wrap: break-word !important;"> 
  
  <p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
    绿盟威胁情报中心（NSFOCUS Threat Intelligence center, NTI）依托公司专业的安全团队和强大的安全研究能力，对全球网络安全威胁和态势进行持续观察和分析，以威胁情报的生产、运营、应用等能力及关键技术作为核心研究内容，推出了绿盟威胁情报平台以及一系列集成威胁情报的新一代安全产品，为用户提供可操作的情报数据、专业的情报服务和高效的威胁防护能力，帮助用户更好地了解和应对各类网络威胁。
  </p></section> </section> </section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;text-align: center;overflow-wrap: break-word !important;"> <section style="padding-right: 1em;padding-left: 1em;max-width: 100%;box-sizing: border-box;display: inline-block;overflow-wrap: break-word !important;">
  
  <span title="" opera-tn-ra-cell="_$.pages:0.layers:0.comps:18.title1" style="padding: 0.3em 0.5em;max-width: 100%;box-sizing: border-box;display: inline-block;border-radius: 0.5em;background-color: rgb(111, 186, 44);font-size: 14px;color: rgb(255, 255, 255);overflow-wrap: break-word !important;"></p> 
  
  <p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
     绿盟科技格物实验室  
  </p>
  
  <p>
    </span></section> <section style="margin-top: -1em;padding: 20px 10px 10px;max-width: 100%;box-sizing: border-box;border-width: 1px;border-style: solid;border-color: rgba(62, 62, 62, 0.17);background-color: rgb(244, 244, 244);overflow-wrap: break-word !important;"> <section powered-by="xiumi.us" style="max-width: 100%;box-sizing: border-box;text-align: justify;font-size: 13px;overflow-wrap: break-word !important;"> 
    
    <p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
      绿盟科技格物实验室专注于工业互联网、物联网和车联网三大业务场景的安全研究。致力于以场景为导向，智能设备为中心的漏洞挖掘、研究与安全分析，关注物联网资产、漏洞、威胁分析。目前已发布多篇研究报告，包括《物联网安全白皮书》、《物联网安全年报2017》、《物联网安全年报2018》、《物联网安全年报2019》、《国内物联网资产的暴露情况分析》、《智能设备安全分析手册》等。与产品团队联合推出绿盟物联网安全风控平台，定位运营商行业物联网卡的风险管控；推出固件安全检测平台，以便快速发现设备中可能存在的漏洞，以避免因弱口令、溢出等漏洞引起设备控制权限的泄露。
    </p></section> </section> </section> <section powered-by="xiumi.us" style="margin-top: 10px;margin-bottom: 10px;max-width: 100%;box-sizing: border-box;text-align: center;overflow-wrap: break-word !important;"> <section style="padding-right: 1em;padding-left: 1em;max-width: 100%;box-sizing: border-box;display: inline-block;overflow-wrap: break-word !important;">
    
    <span title="" opera-tn-ra-cell="_$.pages:0.layers:0.comps:19.title1" style="padding: 0.3em 0.5em;max-width: 100%;box-sizing: border-box;display: inline-block;border-radius: 0.5em;background-color: rgb(111, 186, 44);font-size: 14px;color: rgb(255, 255, 255);overflow-wrap: break-word !important;"></p> 
    
    <p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
      绿盟科技伏影实验室
    </p>
    
    <p>
      </span></section> <section style="margin-top: -1em;padding: 20px 10px 10px;max-width: 100%;box-sizing: border-box;border-width: 1px;border-style: solid;border-color: rgba(62, 62, 62, 0.17);background-color: rgb(244, 244, 244);overflow-wrap: break-word !important;"> <section powered-by="xiumi.us" style="max-width: 100%;box-sizing: border-box;text-align: justify;font-size: 13px;overflow-wrap: break-word !important;"> 
      
      <p style="max-width: 100%;box-sizing: border-box;min-height: 1em;overflow-wrap: break-word !important;">
        绿盟科技伏影实验室专注于安全威胁与监测技术研究。研究目标包括僵尸网络威胁，DDoS对抗，WEB对抗，流行服务系统脆弱利用威胁、身份认证威胁，数字资产威胁，黑色产业威胁及新兴威胁。通过掌控现网威胁来识别风险，缓解威胁伤害，为威胁对抗提供决策支撑。
      </p></section> </section> </section> </section>