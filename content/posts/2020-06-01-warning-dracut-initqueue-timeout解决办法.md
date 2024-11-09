---
title: 'Warning: dracut-initqueue timeout解决办法'
author: Kubehan
type: post
date: 2020-06-01T13:07:09+08:00
url: /2616.html
Baidusubmit:
  - 1
views:
  - 14322
cms_top:
  - 'true'
cat_top:
  - 'true'
hot:
  - 'true'
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/06/image-1591016225624.png
zm_like:
  - 4
baidu_record:
  - 1
bigger_cover:
  - https://www.kubehan.cn/wp-content/uploads/posterimg/poster-2616.png
categories:
  - Linux运维

---
最近在IBM上安装系统时出现了这么个情况超时的情况  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/06/image-1591016198733.png" alt="file" /> 

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/06/image-1591016225624.png" alt="file" /> 

总结网上的解决办法：  
网上大多数教程都是这样的

<pre><code class="language-bash">dracut:/# cd dev
dracut:/# ls |grep sdb
这样子你就会看到所有的设备信息。找到sdbx,x为一个数字，是你u盘所在
dracut:/# reboot</code></pre>

重启之后，在install页面按e键修改

vmlinuz initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64.check quiet为 vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sdbx(你u盘所在)quiet

然后按Ctrl+x保存就好了。但是在操作过程中，我发现的dev里面的sdb开头的只有sdb,sdb1和sdb2，于是我就把它仨都试了一遍，就过都说找不到img文件。  
本来我以为我的电脑不能安装Linux的，后来我发现他们一般都说默认是sdb4，可我的dev里面没有sdb4，不过我的dev有个sdc4，于是我就使用sdc4  
修改

vmlinuz initrd=initrd.img inst.stage2=hd:LABEL=CentOS\x207\x20x86_64.check quiet  
为

vmlinuz initrd=initrd.img inst.stage2=hd:/dev/sdc4 quiet

弊端：对小白来说，找到自己的U盘的表示符难免会出错，

#### 我的解决办法：原理都一样

1.ISO下，在 /isolinux/isolinux.cfg文件中  
将原来的U盘标签 “CentOS 7 x86_64” 全部替换为自己设定的U盘标签，强烈建议U盘标签从简，不然一不小心就出错了

里面的inst.stage2=hd:LABEL=CentOS\x207\x20x86_64  
这就是造成超时的原因，inst.stage2 这里应该是指向一个具体的地址，如果是DVD，它的标签就是“CentOS 7 x86_64”，而U盘则可能是你自己定义的标签。 这就造成了DVD能正常安装，U盘就不行了。

2.在选择安装CentOS时，选择 Install CentOS 7 ,然后修改 按 e 键，进入修改状态，将 hd:LABEL= 修改为U盘的标签

OK，到这里就能正确安装了