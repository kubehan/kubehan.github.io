---
title: elasticsearch启动常见错误
author: Kubehan
type: post
date: 2020-03-28T03:19:32+08:00
url: /2033.html
views:
  - 921
  - 921
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032820_0320_elasticsear3.png
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032820_0320_elasticsear3.png
categories:
  - Linux运维

---
<span style="font-size: 14pt;"><span style="color: black; font-family: Arial;">问题出现环境，OS版本：CentOS-7-x86_64-Minimal-1708；ES版本：elasticsearch-6.2.2。</span><br /> </span>

<span style="font-size: 14pt;"><span style="color: black; font-family: Arial;">1、max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]</span><br /> </span>

<span style="font-size: 14pt;"><span style="color: black; font-family: Arial;">　　每个进程最大同时打开文件数太小，可通过下面2个命令查看当前数量</span><br /> </span>

`<span style="color: black; font-family: Courier New; font-size: 16pt; background-color: whitesmoke;">ulimit -Hn<br />
</span>`

`<span style="font-family: Courier New; font-size: 14pt;"><span style="color: black; background-color: whitesmoke;">ulimit -Sn</span></span>`

<span style="font-size: 14pt;"><span style="color: black; font-family: Arial;">　　修改/etc/security/limits.conf文件，增加配置，用户退出后重新登录生效</span><br /> </span>

    <span style="color: black; font-family: Courier New; font-size: 16pt; background-color: whitesmoke;">*  soft    nofile          65536
    </span>

    <span style="font-family: Courier New; font-size: 16pt;"><span style="color: black; background-color: whitesmoke;">*  hard    nofile          65536</span></span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032820_0320_elasticsear1.png" alt="" /> <span style="font-size: 12pt;"><br /> </span>

<span style="font-size: 16pt;"><span style="color: black; font-family: Arial;">2、max number of threads [3818] for user [es] is too low, increase to at least [4096]</span><br /> </span>

<span style="font-size: 16pt;"><span style="color: black; font-family: Arial;">　　问题同上，最大线程个数太低。修改配置文件/etc/security/limits.conf，增加配置</span><br /> </span>

<div>
  <table style="border-collapse: collapse; height: 67px;" border="0" width="527">
    <colgroup> <col style="width: 35px;" /> <col style="width: 287px;" /></colgroup> <tr>
      <td style="border: solid silver 0.5pt; padding: 3px;">
        <p style="text-align: right; background: #f4f4f4;">
          <span style="color: #afafaf; font-family: Consolas; font-size: 7pt;">1<br /> </span>
        </p>
        
        <p style="text-align: right; background: white;">
          <span style="color: #afafaf; font-family: Consolas; font-size: 7pt;">2</span>
        </p>
      </td>
      
      <td style="border-top: solid silver 0.5pt; border-left: none; border-bottom: solid silver 0.5pt; border-right: solid silver 0.5pt; padding: 3px;">
        <p style="background: #f4f4f4;">
          <span style="font-family: Consolas; font-size: 7pt;"><span style="color: black;">*               soft    nproc           4096</span><br /> </span>
        </p>
        
        <p style="background: white;">
          <span style="color: black; font-family: Consolas; font-size: 7pt;">*               hard    nproc           4096</span>
        </p>
      </td>
    </tr>
  </table>
</div>

<span style="color: black; font-family: Arial; font-size: 8pt;">　　可通过命令查看</span><span style="font-size: 12pt;"><br /> </span>

    <span style="color: black; font-family: Courier New; font-size: 16pt; background-color: whitesmoke;">ulimit -Hu
    </span>

    <span style="font-family: Courier New; font-size: 16pt;"><span style="color: black; background-color: whitesmoke;">ulimit -Su</span></span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032820_0320_elasticsear2.png" alt="" /> <span style="font-size: 12pt;"><br /> </span>

<span style="font-size: 16pt;"><span style="color: black; font-family: Arial;">3、max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]</span><br /> </span>

<span style="font-size: 16pt;"><span style="color: black; font-family: Arial;">　　修改/etc/sysctl.conf文件，增加配置vm.max_map_count=262144</span><br /> </span>

    <span style="font-family: Courier New;"><span style="color: black;"><span style="font-size: 16pt; background-color: whitesmoke;">vi /etc/sysctl.conf</span><span style="font-size: 7pt; background-color: whitesmoke;">
    </span><span style="font-size: 14pt; background-color: whitesmoke;">sysctl -p</span></span></span>

<span style="font-size: 14pt;"><span style="color: black; font-family: Arial;">　　执行命令sysctl -p生效</span><br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032820_0320_elasticsear3.png" alt="" /> <span style="font-size: 12pt;"><br /> </span>

<span style="font-size: 14pt;"><span style="color: black; font-family: Arial;"> 4、Exception in thread "main" java.nio.file.AccessDeniedException: /usr/local/elasticsearch/elasticsearch-6.2.2-1/config/jvm.options</span><br /> </span>

<span style="font-size: 14pt;"><span style="color: black; font-family: Arial;">　　elasticsearch用户没有该文件夹的权限，执行命令</span><br /> </span>

    <span style="font-family: Courier New; font-size: 14pt;"><span style="color: black; background-color: whitesmoke;">chown -R es:es /usr/local/elasticsearch/</span>				</span>