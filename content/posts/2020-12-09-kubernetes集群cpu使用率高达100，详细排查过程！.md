---
title: Kubernetes集群CPU使用率高达100%，详细排查过程！
author: Kubehan
type: post
date: 2020-12-09T03:54:17+08:00
url: /3115.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/10/1603788342-d03840c86350822.png
views:
  - 1002
categories:
  - Linux运维

---
<section data-mpa-powered-by="yiban.io"> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="text-align: right;justify-content: flex-end;margin: 10px 8px 15px;box-sizing: border-box;line-height: 2em;"> <section style="display: inline-block;width: 98%;vertical-align: top;border-style: solid;border-width: 2px;border-radius: 0px;border-color: rgb(178, 211, 218);height: auto;padding: 8px 0px 0px;box-sizing: border-box;"> <section style="margin: 0px 0%;box-sizing: border-box;" powered-by="xiumi.us"> <section style="font-size: 17px;color: rgb(106, 106, 106);font-family: Optima-Regular, PingFangTC-light;padding: 0px 10px;letter-spacing: 0px;box-sizing: border-box;"> 

<p style="text-align: justify;white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <strong style="box-sizing: border-box;">作者介绍</strong>
</p></section> </section> <section style="transform: translate3d(-8px, 0px, 0px);-webkit-transform: translate3d(-8px, 0px, 0px);-moz-transform: translate3d(-8px, 0px, 0px);-o-transform: translate3d(-8px, 0px, 0px);margin: 0px 0% -8px;text-align: left;justify-content: flex-start;box-sizing: border-box;" powered-by="xiumi.us"> <section style="display: inline-block;width: 100%;vertical-align: top;border-width: 0px;background-color: rgba(113, 178, 198, 0.25);box-shadow: rgb(0, 0, 0) 0px 0px 0px;padding: 10px;box-sizing: border-box;"> <section style="margin: 0px 0%;box-sizing: border-box;" powered-by="xiumi.us"> <section style="text-align: justify;font-size: 14px;color: rgb(106, 106, 106);line-height: 1.8;letter-spacing: 1.8px;padding: 0px;box-sizing: border-box;"> 

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  谢泽钦，Rancher Labs技术支持工程师，负责Rancher中国客户的维护和相关技术支持。4年的云计算领域的经验，经历了OpenStack到Kubernetes的技术变革，对底层操作系统、KVM虚拟化和Docker容器等相关云技术拥有丰富的运维和实践经验。
</p></section> </section> </section> </section> </section> </section> <section style="margin: 10px 8px 8px;box-sizing: border-box;line-height: 2em;"> <section style="display: inline-block;width: 100%;vertical-align: top;border-left: 3px solid rgb(160, 160, 160);border-bottom-left-radius: 0px;padding: 0px 0px 0px 8px;background-color: rgb(250, 250, 250);box-sizing: border-box;"> <section style="color: rgb(117, 117, 117);box-sizing: border-box;" powered-by="xiumi.us"> 

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 15px;">本文是为Rancher客户进行问题排查的记录。</span>
</p></section> </section> </section> <section> <section> <section powered-by="xiumi.us"> <section>

<br mpa-from-tpl="t" /> </section> <section data-mpa-template="t" mpa-from-tpl="t"> <section data-mpa-template="t" mpa-from-tpl="t"> 

<p data-lake-id="211f5af6489e4a3d9ec72c47be122449" style="clear: both;min-height: 1em;text-align: justify;font-size: 15px;color: rgb(64, 64, 64);line-height: 2;letter-spacing: 0.008em;outline-style: none;">
</p>

<h1 data-lake-id="551a1f71e68241ac0261e393779344ef" style="font-weight: 400;font-size: 16px;color: rgb(51, 51, 51);text-align: justify;line-height: 1.75em;">
  <span style="color: rgb(2, 30, 170);"><strong mpa-from-tpl="t"><span style="font-size: 17px;">问题背景</span></strong></span>
</h1><article tabindex="0" style="color: rgb(51, 51, 51);font-size: 17px;text-align: justify;"> <article tabindex="0"> 

<hr style="letter-spacing: 0.544px;border-style: solid;border-right-width: 0px;border-bottom-width: 0px;border-left-width: 0px;border-color: rgba(0, 0, 0, 0.1);transform-origin: 0px 0px 0px;transform: scale(1, 0.5);" />
</article> </article> 

<p style="clear: both;min-height: 1em;color: rgb(51, 51, 51);font-size: 17px;text-align: justify;">
</p></section> </section> </section> </section> </section> <section powered-by="xiumi.us"> 

<p style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">
  <span style="font-size: 15px;">我们发现客户的Kubernetes集群环境中所有的worker节点的Kubelet进程的CPU使用率长时间占用过高，通过pidstat可以看到CPU使用率高达100%。本文记录下了本次问题排查的过程。<br style="box-sizing: border-box;" /></span>
</p></section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> 

<p style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">
</p></section> <section style="text-align: center;margin: 10px 8px;box-sizing: border-box;line-height: 2em;"> <section style="max-width: 100%;vertical-align: middle;display: inline-block;line-height: 0;box-sizing: border-box;">

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-9792e2b4df7dd05af2434f3355242371.png" /> </section> </section> <section powered-by="xiumi.us"> <section style="line-height: 2em;"></section> <section data-mpa-template="t" mpa-from-tpl="t"> <section data-mpa-template="t" mpa-from-tpl="t"> 

<p data-lake-id="211f5af6489e4a3d9ec72c47be122449" style="clear: both;min-height: 1em;text-align: justify;font-size: 15px;color: rgb(64, 64, 64);line-height: 2;letter-spacing: 0.008em;outline-style: none;">
</p>

<h1 data-lake-id="551a1f71e68241ac0261e393779344ef" style="font-weight: 400;font-size: 16px;color: rgb(51, 51, 51);text-align: justify;line-height: 2em;">
  <span style="color: rgb(2, 30, 170);font-size: 18px;"><strong mpa-from-tpl="t">集群环境</strong></span>
</h1><article tabindex="0" style="color: rgb(51, 51, 51);font-size: 17px;text-align: justify;"> <article tabindex="0"> 

<hr style="letter-spacing: 0.544px;border-style: solid;border-right-width: 0px;border-bottom-width: 0px;border-left-width: 0px;border-color: rgba(0, 0, 0, 0.1);transform-origin: 0px 0px 0px;transform: scale(1, 0.5);" />
</article> </article> </section> </section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="min-height: 40px;margin: 10px 0%;box-sizing: border-box;" powered-by="xiumi.us"> <section style="width: 100%;margin: 0px auto -10px;box-sizing: border-box;"> 

<table style="border-collapse: collapse;box-sizing: border-box;margin-bottom: 10px;" width="100%">
  <tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:16.classicTable1:0" style="box-sizing: border-box;" powered-by="xiumi.us">
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:0.td@@0" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">软件</span></section> </section>
    </td>
    
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:0.td@@1" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">版本</span></section> </section>
    </td>
  </tr>
  
  <tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:16.classicTable1:1" style="box-sizing: border-box;" powered-by="xiumi.us">
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:1.td@@0" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">Kubernetes</span></section> </section>
    </td>
    
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:1.td@@1" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="text-align: center;white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">v1.18.9<br style="box-sizing: border-box;" /></span></section> </section>
    </td>
  </tr>
  
  <tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:16.classicTable1:2" style="box-sizing: border-box;" powered-by="xiumi.us">
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:2.td@@0" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">Docker</span></section> </section>
    </td>
    
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:2.td@@1" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">18.09.9</span></section> </section>
    </td>
  </tr>
  
  <tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:16.classicTable1:3" style="box-sizing: border-box;" powered-by="xiumi.us">
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:3.td@@0" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">Rancher</span></section> </section>
    </td>
    
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:3.td@@1" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">v2.4.8-ent</span></section> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">（社区版同样适用）</span></section> </section>
    </td>
  </tr>
  
  <tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:16.classicTable1:4" style="box-sizing: border-box;" powered-by="xiumi.us">
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:4.td@@0" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">CentOS</span></section> </section>
    </td>
    
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:4.td@@1" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">7.6</span></section> </section>
    </td>
  </tr>
  
  <tr opera-tn-ra-comp="_$.pages:0.layers:0.comps:16.classicTable1:5" style="box-sizing: border-box;" powered-by="xiumi.us">
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:5.td@@0" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="text-align: center;box-sizing: border-box;" powered-by="xiumi.us"> <section style="margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">Kernel</span></section> </section>
    </td>
    
    <td colspan="1" rowspan="1" opera-tn-ra-cell="_$.pages:0.layers:0.comps:16.classicTable1:5.td@@1" style="border-width: 1px;border-color: rgb(62, 62, 62);border-radius: 0px;border-style: solid;box-sizing: border-box;padding: 0px;" width="50.0000%">
      <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">4.4.227-1.el7.elrepo.x86_64</span></section> </section>
    </td>
  </tr>
</table></section> </section> <section powered-by="xiumi.us"> <section style="line-height: 2em;"></section> <section data-mpa-template="t" mpa-from-tpl="t"> <section data-mpa-template="t" mpa-from-tpl="t"> 

<p data-lake-id="211f5af6489e4a3d9ec72c47be122449" style="clear: both;min-height: 1em;text-align: justify;font-size: 15px;color: rgb(64, 64, 64);line-height: 2;letter-spacing: 0.008em;outline-style: none;">
</p>

<h1 data-lake-id="551a1f71e68241ac0261e393779344ef" style="font-weight: 400;font-size: 16px;color: rgb(51, 51, 51);text-align: justify;line-height: 2em;">
  <span style="color: rgb(2, 30, 170);font-size: 18px;"><strong mpa-from-tpl="t">排查过程</strong></span>
</h1><article tabindex="0" style="color: rgb(51, 51, 51);font-size: 17px;text-align: justify;"> <article tabindex="0"> 

<hr style="letter-spacing: 0.544px;border-style: solid;border-right-width: 0px;border-bottom-width: 0px;border-left-width: 0px;border-color: rgba(0, 0, 0, 0.1);transform-origin: 0px 0px 0px;transform: scale(1, 0.5);" />
</article> </article> </section> </section> <section style="line-height: 2em;"></section> </section> <section style="margin: 10px 8px;text-align: left;box-sizing: border-box;line-height: 2em;"> <section style="display: inline-block;vertical-align: top;box-sizing: border-box;"> <section style="display: inline-block;vertical-align: middle;color: rgb(0, 117, 168);box-sizing: border-box;"> 

<p style="margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="color: rgb(2, 30, 170);font-size: 15px;">使用strace工具对kubelet进程进行跟踪</span>
</p></section> <section style="width: 0px;border-left: 8px solid rgb(0, 117, 168);border-top: 5px solid transparent;border-bottom: 5px solid transparent;margin-right: 3px;margin-left: 5px;display: inline-block;vertical-align: middle;box-sizing: border-box;"> <section><svg viewbox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section> </section> <section style="width: 0px;border-left: 8px solid rgb(255, 255, 255);border-top: 4px solid transparent;border-bottom: 4px solid transparent;display: inline-block;vertical-align: middle;margin-right: -9px;transform: rotate(0deg);-webkit-transform: rotate(0deg);-moz-transform: rotate(0deg);-o-transform: rotate(0deg);box-sizing: border-box;"> <section><svg viewbox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section> </section> </section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">1、由于Kubelet进程CPU使用率异常，可以使用strace工具对kubelet进程动态跟踪进程的调用情况，首先使用<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">strace -cp <pid></pid></span>命令统计kubelet进程在某段时间内的每个系统调用的时间、调用和错误情况。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="text-align: center;margin: 10px 8px;box-sizing: border-box;line-height: 2em;"> <section style="max-width: 100%;vertical-align: middle;display: inline-block;line-height: 0;box-sizing: border-box;"><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-e31a88d1d3a52093e6b3b10266a6390e.png" /></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">从上图可以看到，执行系统调用过程中，futex抛出了五千多个errors，这并不是一个正常的数量，而且该函数占用的时间达到了99%，所以需要进一步查看kubelet进程相关的调用。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">2、由于<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">strace -cp</span>命令只能查看进程的整体调用情况，所以我们可以通过<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">strace -tt -p <pid></pid></span>命令打印每个系统调用的时间戳，如下：</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="text-align: center;margin-left: 8px;margin-right: 8px;line-height: 2em;"><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-bc46c6683fec662c1c866a1c4182a92b.png" /></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">从strace输出的结果来看，在执行futex相关的系统调用时，有大量的<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">Connect timed out</span>，并返回了<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">-1</span>和<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">ETIMEDOUT</span>的error，所以才会在<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">strace -cp</span>中看到了那么多的error。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">futex是一种用户态和内核态混合的同步机制，当futex变量告诉进程有竞争发生时，会执行系统调用去完成相应的处理，例如<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">wait</span>或者<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">wake up</span>，从官方的文档了解到，futex有这么几个参数：</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section class="code-snippet\_\_fix code-snippet\_\_js"> 

<ul class="code-snippet__line-index code-snippet__js">
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
</ul>

<pre class="code-snippet__js" data-lang="cpp">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">futex(uint32_t *uaddr, int futex_op, uint32_t val,&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">const struct timespec *timeout,   /* or: uint32_t val2 */&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">uint32_t *uaddr2, uint32_t val3);&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">官方文档给出<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">ETIMEDOUT</span>的解释：</span></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section class="code-snippet\_\_fix code-snippet\_\_js"> 

<ul class="code-snippet__line-index code-snippet__js">
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
</ul>

<pre class="code-snippet__js" data-lang="python">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">ETIMEDOUT&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">       The operation in futex_op employed the timeout specified in&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">       timeout, and the timeout expired before the operation&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">       completed.&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="text-align: left;white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">意思就是在指定的timeout时间中，未能完成相应的操作，其中<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">futex_op</span>对应上述输出结果的<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">FUTEX_WAIT_PRIVATE</span>和<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">FUTEX_WAIT_PRIVATE</span>，可以看到基本都是发生在<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">FUTEX_WAIT_PRIVATE</span>时发生的超时。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="text-align: left;white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">从目前的系统调用层面可以判断，futex无法顺利进入睡眠状态，但是futex进行了哪些操作还是不清楚，因此仍无法判断kubeletCPU飙高的原因，所以我们需要进一步从kubelet的函数调用中去看到底是执行卡在了哪个地方。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="margin: 10px 8px 8px;box-sizing: border-box;line-height: 2em;"> <section style="display: inline-block;width: 100%;vertical-align: top;border-left: 3px solid rgb(160, 160, 160);border-bottom-left-radius: 0px;padding: 0px 0px 0px 8px;background-color: rgb(250, 250, 250);box-sizing: border-box;"> <section style="color: rgb(117, 117, 117);box-sizing: border-box;" powered-by="xiumi.us"> 

<p style="text-align: left;white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 12px;">FUTEX_PRIVATE_FLAG：这个参数告诉内核futex是进程专用的，不与其他进程共享，这里的FUTEX_WAIT_PRIVATE和FUTEX_WAKE_PRIVATE就是其中的两种FLAG；</span>
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <br style="box-sizing: border-box;" />
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 12px;">futex相关说明1：</span>
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 12px;">https://man7.org/linux/man-pages/man7/futex.7.html</span>
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 12px;">fuex相关说明2： </span>
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 12px;">https://man7.org/linux/man-pages/man2/futex.2.html</span>
</p></section> </section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="margin: 10px 8px;text-align: left;box-sizing: border-box;line-height: 2em;"> <section style="display: inline-block;vertical-align: top;box-sizing: border-box;"> <section style="display: inline-block;vertical-align: middle;color: rgb(0, 117, 168);box-sizing: border-box;"> 

<p style="margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 15px;color: rgb(2, 30, 170);">使用go pprof工具对kubelet函数调用进行分析</span>
</p></section> <section style="width: 0px;border-left: 8px solid rgb(0, 117, 168);border-top: 5px solid transparent;border-bottom: 5px solid transparent;margin-right: 3px;margin-left: 5px;display: inline-block;vertical-align: middle;box-sizing: border-box;"> <section><svg viewbox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section> </section> <section style="width: 0px;border-left: 8px solid rgb(255, 255, 255);border-top: 4px solid transparent;border-bottom: 4px solid transparent;display: inline-block;vertical-align: middle;margin-right: -9px;transform: rotate(0deg);-webkit-transform: rotate(0deg);-moz-transform: rotate(0deg);-o-transform: rotate(0deg);box-sizing: border-box;"> <section><svg viewbox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section> </section> </section> <section style="display: inline-block;vertical-align: top;box-sizing: border-box;"> <section style="width: 0px;border-left: 10px solid rgb(0, 117, 168);border-top: 6px solid transparent;border-bottom: 6px solid transparent;display: inline-block;vertical-align: middle;box-sizing: border-box;"> <section><svg viewbox="0 0 1 1" style="float:left;line-height:0;width:0;vertical-align:top;"></svg></section> </section> </section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="text-align: left;white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">早期的Kubernetes版本，可以直接通过debug/pprof 接口获取debug数据，后面考虑到相关安全性的问题，取消了这个接口，具体信息可以参考CVE-2019-11248（<span style="text-decoration: underline;box-sizing: border-box;">https://github.com/kubernetes/kubernetes/issues/81023</span>）。因此我们将通过kubectl开启proxy进行相关数据指标的获取：</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">1、首先使用<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">kubectl proxy</span>命令启动API server代理</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section class="code-snippet\_\_fix code-snippet\_\_js"> 

<ul class="code-snippet__line-index code-snippet__js">
  <li>
    </ul> <pre class="code-snippet__js" data-lang="nginx">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">kubectl proxy --address='0.0.0.0'  --accept-hosts='^*$'&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">这里需要注意，如果使用的是Rancher UI上复制的kubeconfig文件，则需要使用指定了master IP的context，如果是RKE或者其他工具安装则可以忽略。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">2、构建Golang环境。go pprof需要在golang环境下使用，本地如果没有安装golang，则可以通过Docker快速构建Golang环境</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section class="code-snippet__fix code-snippet__js"> <ul class="code-snippet__line-index code-snippet__js">
  <li>
    </ul> <pre class="code-snippet__js" data-lang="nginx">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">docker run -itd --name golang-env --net host golang bash&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">3、使用go pprof工具导出采集的指标，这里替换127.0.0.1为apiserver节点的IP，默认端口是8001，如果docker run的环境跑在apiserver所在的节点上，可以使用127.0.0.1。另外，还要替换NODENAME为对应的节点名称。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section class="code-snippet__fix code-snippet__js"> <ul class="code-snippet__line-index code-snippet__js">
  <li>
  </li>
  <li>
  </li>
</ul>

<pre class="code-snippet__js" data-lang="ruby">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">docker exec -it golang-env bash&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">go tool pprof -seconds=60 -raw -output=kubelet.pprof http://127.0.0.1:8001/api/v1/nodes/${NODENAME}/proxy/debug/pprof/profile&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">4、输出好的pprof文件不方便查看，需要转换成火焰图，推荐使用FlameGraph工具生成svg图</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section class="code-snippet__fix code-snippet__js"> <ul class="code-snippet__line-index code-snippet__js">
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
</ul>

<pre class="code-snippet__js" data-lang="cs">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">git clone https://github.com/brendangregg/FlameGraph.git&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">cd FlameGraph/&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">./stackcollapse-go.pl kubelet.pprof &gt; kubelet.out&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">./flamegraph.pl kubelet.out &gt; kubelet.svg&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">转换成火焰图后，就可以在浏览器直观地看到函数相关调用和具体调用时间比了。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">5、分析火焰图</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="text-align: center;margin: 10px 8px;box-sizing: border-box;line-height: 2em;"> <section style="max-width: 100%;vertical-align: middle;display: inline-block;line-height: 0;box-sizing: border-box;"><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-6e4888e652e9000635a6deb739138e94.png" /></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">从kubelet的火焰图可以看到，调用时间最长的函数是<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">k8s.io/kubernetes/vendor/github.com/google/cadvisor/manager.(*containerData).housekeeping</span>，其中cAdvisor是kubelet内置的指标采集工具，主要是负责对节点机器上的资源及容器进行实时监控和性能数据采集，包括CPU使用情况、内存使用情况、网络吞吐量及文件系统使用情况。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="text-align: left;white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">深入函数调用可以发现<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">k8s.io/kubernetes/vendor/github.com/opencontainers/runc/libcontainer/cgroups/fs.(*Manager).GetStats</span>这个函数占用<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">k8s.io/kubernetes/vendor/github.com/google/cadvisor/manager.(*containerData).housekeeping</span>这个函数的时间是最长的，说明在获取容器CGroup相关状态时占用了较多的时间。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">6、既然这个函数占用时间长，那么我们就分析一下这个函数具体干了什么。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">查看源代码：</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="text-decoration: underline;box-sizing: border-box;font-size: 15px;">https://github.com/kubernetes/kubernetes/blob/ded8a1e2853aef374fc93300fe1b225f38f19d9d/vendor/github.com/opencontainers/runc/libcontainer/cgroups/fs/memory.go#L162</span></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section class="code-snippet__fix code-snippet__js"> <ul class="code-snippet__line-index code-snippet__js">
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
</ul>

<pre class="code-snippet__js" data-lang="go">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">func (s *MemoryGroup) GetStats(path string, stats *cgroups.Stats) error {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">// Set stats from memory.stat.&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  statsFile, err := os.Open(filepath.Join(path, "memory.stat"))&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if os.IsNotExist(err) {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return nil&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">    }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return err&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">defer statsFile.Close()&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;br>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  sc := bufio.NewScanner(statsFile)&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">for sc.Scan() {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">    t, v, err := fscommon.GetCgroupParamKeyValue(sc.Text())&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return fmt.Errorf("failed to parse memory.stat (%q) - %v", sc.Text(), err)&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">    }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">    stats.MemoryStats.Stats[t] = v&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  stats.MemoryStats.Cache = stats.MemoryStats.Stats["cache"]&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;br>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  memoryUsage, err := getMemoryData(path, "")&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return err&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  stats.MemoryStats.Usage = memoryUsage&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  swapUsage, err := getMemoryData(path, "memsw")&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return err&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  stats.MemoryStats.SwapUsage = swapUsage&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  kernelUsage, err := getMemoryData(path, "kmem")&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return err&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  stats.MemoryStats.KernelUsage = kernelUsage&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  kernelTCPUsage, err := getMemoryData(path, "kmem.tcp")&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return err&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  stats.MemoryStats.KernelTCPUsage = kernelTCPUsage&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;br>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  useHierarchy := strings.Join([]string{"memory", "use_hierarchy"}, ".")&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  value, err := fscommon.GetCgroupParamUint(path, useHierarchy)&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return err&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if value == 1 {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">    stats.MemoryStats.UseHierarchy = true&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;br>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  pagesByNUMA, err := getPageUsageByNUMA(path)&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">if err != nil {&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return err&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  }&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">  stats.MemoryStats.PageUsageByNUMA = pagesByNUMA&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;br>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">return nil&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">}&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">从代码中可以看到，进程会去读取<span style="color: rgb(0, 117, 168);background-color: rgba(0, 0, 0, 0.05);box-sizing: border-box;">memory.stat</span>这个文件，这个文件存放了cgroup内存使用情况。也就是说，在读取这个文件花费了大量的时间。这时候，如果我们手动去查看这个文件，会是什么效果？</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section class="code-snippet__fix code-snippet__js"> <ul class="code-snippet__line-index code-snippet__js">
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
  <li>
  </li>
</ul>

<pre class="code-snippet__js" data-lang="properties">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;"># time cat /sys/fs/cgroup/memory/memory.stat &gt;/dev/null&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">real 0m9.065s&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">user 0m0.000s&lt;/span>&lt;/span></code><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">sys 0m9.064s&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">从这里可以看出端倪了，读取这个文件花费了9s，显然是不正常的。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;text-align: left;line-height: 2em;"><span style="font-size: 15px;">基于上述结果，我们在cAdvisor的GitHub上查找到一个issue（<span style="text-decoration: underline;box-sizing: border-box;">https://github.com/google/cadvisor/issues/1774</span>），从该issue中可以得知，该问题跟slab memory 缓存有一定的关系。从该issue中得知，受影响的机器的内存会逐渐被使用，通过/proc/meminfo看到使用的内存是slab memory，该内存是内核缓存的内存页，并且其中绝大部分都是dentry缓存。从这里我们可以判断出，当CGroup中的进程生命周期结束后，由于缓存的原因，还存留在slab memory中，导致其类似僵尸CGroup一样无法被释放。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">也就是每当创建一个memory CGroup，在内核内存空间中，就会为其创建分配一份内存空间，该内存包含当前CGroup相关的cache（dentry、inode），也就是目录和文件索引的缓存，该缓存本质上是为了提高读取的效率。但是当CGroup中的所有进程都退出时，存在内核内存空间的缓存并没有清理掉。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">内核通过伙伴算法进行内存分配，每当有进程申请内存空间时，会为其分配至少一个内存页面，也就是最少会分配4k内存，每次释放内存，也是按照最少一个页面来进行释放。当请求分配的内存大小为几十个字节或几百个字节时，4k对其来说是一个巨大的内存空间，在Linux中，为了解决这个问题，引入了slab内存分配管理机制，用来处理这种小量的内存请求，这就会导致，当CGroup中的所有进程都退出时，不会轻易回收这部分的内存，而这部分内存中的缓存数据，还会被读取到stats中，从而导致影响读取的性能。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;"><br /></span></section> <section data-mpa-template="t" mpa-from-tpl="t"> <section data-mpa-template="t" mpa-from-tpl="t"> <p data-lake-id="211f5af6489e4a3d9ec72c47be122449" style="clear: both;min-height: 1em;text-align: justify;font-size: 15px;color: rgb(64, 64, 64);line-height: 2;letter-spacing: 0.008em;outline-style: none;">
  <span style="color: rgb(51, 51, 51);"></span>
</p>

<h1 data-lake-id="551a1f71e68241ac0261e393779344ef" style="font-weight: 400;font-size: 16px;color: rgb(51, 51, 51);text-align: justify;line-height: 1.75em;">
  <span style="color: rgb(2, 30, 170);"><strong mpa-from-tpl="t"><span style="font-size: 17px;">解决方法</span></strong></span>
</h1><article tabindex="0" style="color: rgb(51, 51, 51);font-size: 17px;text-align: justify;"> <article tabindex="0"> 

<hr style="letter-spacing: 0.544px;border-style: solid;border-right-width: 0px;border-bottom-width: 0px;border-left-width: 0px;border-color: rgba(0, 0, 0, 0.1);transform-origin: 0px 0px 0px;transform: scale(1, 0.5);" />
</article> </article> </section> </section> </section> <section style="box-sizing: border-box;margin-left: 8px;margin-right: 8px;line-height: 2em;"> <section style="display: inline-block;vertical-align: top;width: 33.33%;box-sizing: border-box;"> <section powered-by="xiumi.us"></section> </section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">
<span style="font-size: 15px;">1、清理节点缓存，这是一个临时的解决方法，暂时清空节点内存缓存，能够缓解kubelet CPU使用率，但是后面缓存上来了，CPU使用率又会升上来。</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section class="code-snippet__fix code-snippet__js"> <ul class="code-snippet__line-index code-snippet__js">
  <li>
    </ul> <pre class="code-snippet__js" data-lang="ruby">&lt;section style="margin-left: 8px;margin-right: 8px;line-height: 2em;"><code>&lt;span class="code-snippet_outer">&lt;span style="font-size: 15px;">echo 2 &gt; /proc/sys/vm/drop_caches&lt;/span>&lt;/span></code>&lt;/section></pre></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> </section> <section style="box-sizing: border-box;" powered-by="xiumi.us"> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;">

<span style="font-size: 15px;">2、升级内核版本</span></section> <section style="white-space: normal;margin: 0px 8px;padding: 0px;box-sizing: border-box;line-height: 2em;"></section> <ul class="list-paddingleft-2" style="list-style-type: disc;box-sizing: border-box;margin-left: 8px;margin-right: 8px;">
  <li style="box-sizing: border-box;font-size: 15px;">
    <section style="margin: 0px;padding: 0px;box-sizing: border-box;text-align: left;line-height: 2em;"><span style="font-size: 15px;">其实这个主要还是内核的问题，在GitHub上这个commit(<span style="text-decoration: underline;box-sizing: border-box;">https://github.com/torvalds/linux/commit/205b20cc5a99cdf197c32f4dbee2b09c699477f0</span>)中有提到，在5.2+以上的内核版本中，优化了CGroup stats相关的查询性能，如果想要更好的解决该问题，建议可以参考自己操作系统和环境，合理的升级内核版本。</span></section>
  </li>
  <li style="box-sizing: border-box;font-size: 15px;">
    <section style="text-align: left;margin: 0px;padding: 0px;box-sizing: border-box;line-height: 2em;"><span style="font-size: 15px;">另外Redhat在kernel-4.18.0-176（<span style="text-decoration: underline;box-sizing: border-box;">https://bugzilla.redhat.com/show_bug.cgi?id=1795049</span>）版本中也优化了相关CGroup的性能问题，而CentOS 8/RHEL 8默认使用的内核版本就是4.18，如果目前您使用的操作系统是RHEL7/CentOS7，则可以尝试逐渐替换新的操作系统，使用这个4.18.0-176版本以上的内核，毕竟新版本内核总归是对容器相关的体验会好很多。</span></section>
  </li>
</ul></section> <section style="margin: 10px 8px 8px;box-sizing: border-box;line-height: 2em;"> <section style="display: inline-block;width: 100%;vertical-align: top;border-left: 3px solid rgb(160, 160, 160);border-bottom-left-radius: 0px;padding: 0px 0px 0px 8px;background-color: rgb(250, 250, 250);box-sizing: border-box;"> <section style="color: rgb(117, 117, 117);box-sizing: border-box;" powered-by="xiumi.us"> 

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 12px;">kernel相关commit：</span>
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="text-decoration: underline;box-sizing: border-box;font-size: 12px;">https://github.com/torvalds/linux/commit/205b20cc5a99cdf197c32f4dbee2b09c699477f0</span>
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="font-size: 12px;">redhat kernel bug fix：</span>
</p>

<p style="white-space: normal;margin: 0px;padding: 0px;box-sizing: border-box;">
  <span style="text-decoration: underline;box-sizing: border-box;font-size: 12px;">https://bugzilla.redhat.com/show_bug.cgi?id=1795049</span>
</p></section> </section> </section> <section> <section> 

<p style="margin-left: 8px;margin-right: 8px;">
  <span style="font-size: 15px;">文章来源：Rancher Labs / </span><a target="_blank" href="https://mp.weixin.qq.com/s?__biz=MzIyMTUwMDMyOQ==&mid=2247495070&idx=1&sn=22d9c5b181daf389e397cd7a8c71cab2&chksm=e8396b58df4ee24e3a3ba56d7a65d1cb63841143512c5ee37a0fbf878c333e763b64a6e0dc5e&token=75760526&lang=zh_CN&scene=21#wechat_redirect" textvalue="原文链接" tab="innerlink" style="font-size: 15px;" data-linktype="2" rel="noopener noreferrer"><span style="font-size: 15px;">原文链接</span></a>
</p></section> <section style="display: inline-block;vertical-align: middle;width: 10%;box-sizing: border-box;"> <section style="text-align: center;margin: 0px 0%;box-sizing: border-box;" powered-by="xiumi.us"></section> </section> </section> <section style='font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif;white-space: normal;color: rgb(89, 89, 89);font-size: 14px;letter-spacing: 0.544px;line-height: 1.75em;text-align: center;'>

<span style="font-size: 15px;color: rgb(85, 85, 85);letter-spacing: 0.544px;widows: 1;caret-color: rgb(51, 51, 51);">END</span></section> <hr style="margin-top: 28px;font-size: 16px;white-space: normal;letter-spacing: 0.544px;text-align: center;widows: 1;caret-color: rgb(63, 63, 63);color: rgb(63, 63, 63);font-family: Avenir, -apple-system-font, 微软雅黑, sans-serif;text-size-adjust: auto;height: 1px;border-top-style: dotted;border-top-color: rgb(165, 165, 165);" />
<section style='margin-top: 15px;margin-bottom: 10px;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif;white-space: normal;color: rgb(89, 89, 89);font-size: 14px;min-height: 1em;letter-spacing: 0.544px;text-align: center;'>
<span style='font-size: 15px;font-family: mp-quote, -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;text-align: justify;'>Kubernetes CKA实战培训班推荐：</span></section> <p style='margin-bottom: 10px;letter-spacing: 1.5px;white-space: normal;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: center;'>
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI5ODQ2MzI3NQ==&mid=2247495347&idx=3&sn=911161521603cd7a6f34d422104b042b&chksm=eca7d7f7dbd05ee16702eba7a931acb1cb099630f6ca60ed00f16b00f3569408e20eea20a43f&scene=21#wechat_redirect" textvalue="北京：12月18-20日" data-itemshowtype="0" tab="innerlink" data-linktype="2" rel="noopener noreferrer">北京：12月18-20日</a>
</p><section style='margin-bottom: 10px;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif;white-space: normal;color: rgb(89, 89, 89);font-size: 14px;min-height: 1em;letter-spacing: 0.544px;text-align: center;'>

<a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI5ODQ2MzI3NQ==&mid=2247495347&idx=3&sn=911161521603cd7a6f34d422104b042b&chksm=eca7d7f7dbd05ee16702eba7a931acb1cb099630f6ca60ed00f16b00f3569408e20eea20a43f&scene=21#wechat_redirect" textvalue="你已选中了添加链接的内容" data-itemshowtype="0" tab="innerlink" data-linktype="1" rel="noopener noreferrer"><span class="js_jump_icon h5_image_link" data-positionback="static" style="inset: auto;"><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-434130ec4e66a5c18c3c995034e66407.jpeg" /></span></a></section> <p style='font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif;white-space: normal;color: rgb(89, 89, 89);font-size: 14px;letter-spacing: 0.7px;text-align: left;'>
</p>

<p style="font-size: 16px;font-variant-numeric: normal;font-variant-east-asian: normal;white-space: normal;color: rgb(89, 89, 89);font-family: 微软雅黑;letter-spacing: 0.544px;widows: 1;text-align: center;line-height: 1.75em;background-color: rgb(255, 255, 255);">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-cef5f2378bda5f41122338c8c8c716dc.gif" />
</p><section data-role="outer" label="Powered by 135editor.com" style="font-size: 16px;font-variant-numeric: normal;font-variant-east-asian: normal;white-space: normal;color: rgb(89, 89, 89);text-align: left;font-family: 微软雅黑;letter-spacing: 0.544px;line-height: 25.6px;widows: 1;background-color: rgb(255, 255, 255);"> <section data-role="outer" label="Powered by 135editor.com"> 

<p style="text-align: center;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-1a7a8139a78ab48a4b92b2d433295aa9.jpeg" />
</p></section> </section> </section>