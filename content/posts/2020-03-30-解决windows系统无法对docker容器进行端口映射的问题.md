---
title: 解决windows系统无法对docker容器进行端口映射的问题
author: Kubehan
type: post
date: 2020-03-30T07:35:19+08:00
url: /2060.html
views:
  - 1227
  - 1227
classic-editor-remember:
  - classic-editor
  - classic-editor
Baidusubmit:
  - 1
  - 1
zm_like:
  - 1
categories:
  - Linux运维

---
<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><strong><span style="font-family: Helvetica;">1</span><span style="font-family: 宋体;">、问题：</span></strong><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">在</span><span style="font-family: Helvetica;">Windows</span><span style="font-family: 宋体;">家庭版下安装了</span><span style="font-family: Helvetica;">docker</span><span style="font-family: 宋体;">，并尝试在其中运行</span><span style="font-family: Helvetica;">jupyter notebook</span><span style="font-family: 宋体;">等服务，但映射完毕之后，在主机的浏览器中，打开</span><span style="font-family: Helvetica;">localhost:port</span><span style="font-family: 宋体;">无法访问对应的服务。</span><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><strong><span style="font-family: Helvetica;">2</span><span style="font-family: 宋体;">、问题出现的原因：</span></strong><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">The reason you're having this, is because on Linux, the docker daemon (and your co<br /> </span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">ntainers) run on the Linux machine itself, so "localhost" is also the host that the container is running on, and the ports are mapped to.<span style="color: black;"><br /> </span></span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">On Windows (and OS X), the docker daemon, and your containers cannot run natively, so only the docker client is running on your Windows machine, but the daemon (and your containers) run in a VirtualBox Virtual Machine, that runs Linux.<br /> </span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">因为</span><span style="font-family: Helvetica;">docker</span><span style="font-family: 宋体;">是运行在</span><span style="font-family: Helvetica;">Linux</span><span style="font-family: 宋体;">上的，在</span><span style="font-family: Helvetica;">Windows</span><span style="font-family: 宋体;">中运行</span><span style="font-family: Helvetica;">docker</span><span style="font-family: 宋体;">，实际上还是在</span><span style="font-family: Helvetica;">Windows</span><span style="font-family: 宋体;">下先安装了一个</span><span style="font-family: Helvetica;">Linux</span><span style="font-family: 宋体;">环境，然后在这个系统中运行的</span><span style="font-family: Helvetica;">docker</span><span style="font-family: 宋体;">。也就是说，服务中使用的</span><span style="font-family: Helvetica;">localhost</span><span style="font-family: 宋体;">指的是这个</span><span style="font-family: Helvetica;">Linux</span><span style="font-family: 宋体;">环境的地址，而不是我们的宿主环境</span><span style="font-family: Helvetica;">Windows</span><span style="font-family: 宋体;">。</span><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><strong><span style="font-family: Helvetica;">3</span><span style="font-family: 宋体;">、解决方法：</span></strong><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">通过命令</span><span style="font-family: Helvetica;">:<br /> </span></span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">docker-machine ip default # 其中，default 是docker-machine的name，可以通过docke<br /> </span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">r-machine -ls 查看<br /> </span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">找到这个</span><span style="font-family: Helvetica;">Linux</span><span style="font-family: 宋体;">的</span><span style="font-family: Helvetica;">ip</span><span style="font-family: 宋体;">地址，一般情况下这个地址是</span><span style="font-family: Helvetica;">192.168.99.100</span><span style="font-family: 宋体;">，然后在</span><span style="font-family: Helvetica;">Windows</span><span style="font-family: 宋体;">的浏览器中，输入这个地址，加上服务的端口即可启用了。</span><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">比如，首先运行一个</span><span style="font-family: Helvetica;">docker </span><span style="font-family: 宋体;">容器：</span><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">docker run -it -p 8888:8888 conda:v1<br /> </span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">其中，</span><span style="font-family: Helvetica;">conda:v1</span><span style="font-family: 宋体;">是我的容器名称。然后在容器中开启</span><span style="font-family: Helvetica;">jupyter notebook </span><span style="font-family: 宋体;">服务：</span><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">jupyter notebook --no-browser --port=8888 --ip=172.17.0.2 --allow-root<br /> </span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">其中的</span><span style="font-family: Helvetica;">ip</span><span style="font-family: 宋体;">参数为我的容器的</span><span style="font-family: Helvetica;">ip</span><span style="font-family: 宋体;">地址，可以通过如下命令获得：</span><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">docker inspect container_id<br /> </span>
</p>

<p style="background: white;">
  <span style="color: #333333; font-size: 10pt;"><span style="font-family: 宋体;">最后在</span><span style="font-family: Helvetica;">windows</span><span style="font-family: 宋体;">浏览器中测试结果：</span><span style="font-family: Helvetica;"><br /> </span></span>
</p>

<p style="background: #a8d08d;">
  <span style="font-family: 宋体; font-size: 10pt;">http://192.168.99.100:8888</span>
</p>