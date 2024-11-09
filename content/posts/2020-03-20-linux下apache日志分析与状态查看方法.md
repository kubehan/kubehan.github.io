---
title: Linux下apache日志分析与状态查看方法
author: Kubehan
type: post
date: 2020-03-20T07:46:56+08:00
url: /1070.html
wb_dl_type:
  - 1
  - 1
views:
  - 2326
  - 2326
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
tags:
  - Apache
  - Windows

---
<p style="text-align:left;line-height:35px">
  <strong><span style="font-size:20px;font-family:&#39;微软雅黑&#39;,sans-serif;color:#333333">Linux</span></strong><strong><span style="font-size:20px;font-family:&#39;微软雅黑&#39;,sans-serif;color:#333333">下apache日志分析与状态查看方法</span></strong>
</p>

<p style="text-align:left;line-height:26px;background:#EBF5FD">
  <span style="font-size:14px;font-family:宋体;color:#333333">使用</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#333333">apache</span><span style="font-size: 14px;font-family:宋体;color:#333333">服务器，有时候需要查看</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#333333">apache</span><span style="font-size: 14px;font-family:宋体;color:#333333">的日志与状态，那么就需要下面的命令了，特分享下方便需要的朋友</span>
</p>

<p style="text-align:left;line-height:30px">
  <strong><span style="font-size: 14px;font-family:宋体;color:#222222">假设</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">apache</span></strong><strong><span style="font-size:14px;font-family:宋体;color:#222222">日志格式为：</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> </span></strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">118.78.199.98 – - [09/Jan/2010:00:59:59 +0800] “GET /Public/Css/index.css HTTP/1.1″ 304 – “http://www.a.cn/common/index.php” “Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; GTB6.3)”</span>
</p>

<p style="text-align:left;line-height:30px">
  <strong><span style="font-size: 14px;font-family:宋体;color:#222222">问题</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">1</span></strong><strong><span style="font-size: 14px;font-family:宋体;color:#222222">：在</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">apachelog</span></strong><strong><span style="font-size:14px;font-family:宋体;color:#222222">中找出访问次数最多的</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">10</span></strong><strong><span style="font-size: 14px;font-family:宋体;color:#222222">个</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">IP</span></strong><strong><span style="font-size: 14px;font-family:宋体;color:#222222">。</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> </span></strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">awk '{print $1}' apache_log |sort |uniq -c|sort -nr|head -n 10</span>
</p>

<p style="text-align:left;line-height:30px">
  <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">awk </span><span style="font-size:14px;font-family:宋体;color:#222222">首先将每条日志中的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">IP</span><span style="font-size:14px;font-family:宋体;color:#222222">抓出来，如日志格式被自定义过，可以</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"> -F </span><span style="font-size:14px;font-family:宋体;color:#222222">定义分隔符和</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"> print</span><span style="font-size: 14px;font-family:宋体;color:#222222">指定列；</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> sort</span><span style="font-size:14px;font-family:宋体;color:#222222">进行初次排序，为的使相同的记录排列到一起；</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> upiq -c </span><span style="font-size:14px;font-family:宋体;color:#222222">合并重复的行，并记录重复次数。</span><span style="font-size: 14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> head</span><span style="font-size:14px;font-family:宋体;color:#222222">进行前十名筛选；</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> sort -nr</span><span style="font-size:14px;font-family:宋体;color:#222222">按照数字进行倒叙排序。</span>
</p>

<p style="text-align:left;line-height:30px">
  <strong><span style="font-size: 14px;font-family:宋体;color:#222222">我参考的命令是：</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> </span></strong><span style="font-size:14px;font-family:宋体;color:#222222">显示</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">10</span><span style="font-size:14px;font-family:宋体;color:#222222">条最常用的命令</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> sed -e "s/| //n/g" ~/.bash_history | cut -d ' ' -f 1 | sort | uniq -c | sort -nr | head</span>
</p>

<p style="text-align:left;line-height:30px">
  <strong><span style="font-size: 14px;font-family:宋体;color:#222222">问题</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">2</span></strong><strong><span style="font-size: 14px;font-family:宋体;color:#222222">：在</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">apache</span></strong><strong><span style="font-size:14px;font-family:宋体;color:#222222">日志中找出访问次数最多的几个分钟。</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> </span></strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">awk '{print&nbsp; $4}' access_log |cut -c 14-18|sort|uniq -c|sort -nr|head<br /> awk </span><span style="font-size:14px;font-family:宋体;color:#222222">用空格分出来的第四列是</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">[09/Jan/2010:00:59:59</span><span style="font-size:14px;font-family:宋体;color:#222222">；</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> cut -c </span><span style="font-size:14px;font-family:宋体;color:#222222">提取</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">14</span><span style="font-size:14px;font-family:宋体;color:#222222">到</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">18</span><span style="font-size:14px;font-family:宋体;color:#222222">个字符</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> </span><span style="font-size:14px;font-family:宋体;color:#222222">剩下的内容和问题</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">1</span><span style="font-size:14px;font-family:宋体;color:#222222">类似。</span>
</p>

<p style="text-align:left;line-height:30px">
  <strong><span style="font-size: 14px;font-family:宋体;color:#222222">问题</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">3</span></strong><strong><span style="font-size: 14px;font-family:宋体;color:#222222">：在</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">apache</span></strong><strong><span style="font-size:14px;font-family:宋体;color:#222222">日志中找到访问最多的页面：</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> </span></strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">awk '{print $11}' apache_log |sed 's/^.*cn/(.*/)/"//1/g'|sort |uniq -c|sort -rn|head</span>
</p>

<p style="text-align:left;line-height:30px">
  <span style="font-size:14px;font-family:宋体;color:#222222">类似问题</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">1</span><span style="font-size:14px;font-family:宋体;color:#222222">和</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">2</span><span style="font-size:14px;font-family:宋体;color:#222222">，唯一特殊是用</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">sed</span><span style="font-size:14px;font-family:宋体;color:#222222">的替换功能将</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">”http://www.a.cn/common/index.php”</span><span style="font-size:14px;font-family:宋体;color:#222222">替换成括号内的内容：</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">”http://www.a.cn</span><span style="font-size:14px;font-family:宋体;color:#222222">（</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">/common/index.php</span><span style="font-size:14px;font-family:宋体;color:#222222">）</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">”</span>
</p>

<p style="text-align:left;line-height:30px">
  <strong><span style="font-size: 14px;font-family:宋体;color:#222222">问题</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">4</span></strong><strong><span style="font-size: 14px;font-family:宋体;color:#222222">：在</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">apache</span></strong><strong><span style="font-size:14px;font-family:宋体;color:#222222">日志中找出访问次数最多（负载最重）的几个时间段（以分钟为单位），然后在看看这些时间哪几个</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">IP</span></strong><strong><span style="font-size: 14px;font-family:宋体;color:#222222">访问的最多？</span></strong><strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> </span></strong><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">1,</span><span style="font-size:14px;font-family:宋体;color:#222222">查看</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">apache</span><span style="font-size: 14px;font-family:宋体;color:#222222">进程</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">:<br /> ps aux | grep httpd | grep -v grep | wc -l</span>
</p>

<p style="text-align:left;line-height:30px">
  <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">2,</span><span style="font-size:14px;font-family:宋体;color:#222222">查看</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">80</span><span style="font-size:14px;font-family:宋体;color:#222222">端口的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">tcp</span><span style="font-size:14px;font-family:宋体;color:#222222">连接</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">:<br /> netstat -tan | grep "ESTABLISHED" | grep ":80" | wc -l</span>
</p>

<p style="text-align:left;line-height:30px">
  <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">3,</span><span style="font-size:14px;font-family:宋体;color:#222222">通过日志查看当天</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">ip</span><span style="font-size:14px;font-family:宋体;color:#222222">连接数，过滤重复</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">:<br /> cat access_log | grep "19/May/2011" | awk '{print $2}' | sort | uniq -c | sort -nr</span>
</p>

<p style="text-align:left;line-height:30px">
  <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">4,</span><span style="font-size:14px;font-family:宋体;color:#222222">当天</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">ip</span><span style="font-size:14px;font-family:宋体;color:#222222">连接数最高的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">ip</span><span style="font-size:14px;font-family:宋体;color:#222222">都在干些什么</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">(</span><span style="font-size:14px;font-family:宋体;color:#222222">原来是蜘蛛</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">):<br /> cat access_log | grep "19/May/2011:00" | grep "61.135.166.230" | awk '{print $8}' | sort | uniq -c | sort -nr | head -n 10</p> 
  
  <p>
    5,</span><span style="font-size:14px;font-family:宋体;color:#222222">当天访问页面排前</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">10</span><span style="font-size:14px;font-family:宋体;color:#222222">的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">url:<br /> cat access_log | grep "19/May/2010:00" | awk '{print $8}' | sort | uniq -c | sort -nr | head -n 10</span>
  </p>
  
  <p style="text-align:left;line-height:30px">
    <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">6,</span><span style="font-size:14px;font-family:宋体;color:#222222">用</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">tcpdump</span><span style="font-size: 14px;font-family:宋体;color:#222222">嗅探</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">80</span><span style="font-size:14px;font-family:宋体;color:#222222">端口的访问看看谁最高</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> tcpdump -i eth0 -tnn dst port 80 -c 1000 | awk -F"." '{print $1"."$2"."$3"."$4}' | sort | uniq -c | sort -nr</span>
  </p>
  
  <p style="text-align:left;line-height:30px">
    <span style="font-size:14px;font-family:宋体;color:#222222">接着从日志里查看该</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">ip</span><span style="font-size:14px;font-family:宋体;color:#222222">在干嘛</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">:<br /> cat access_log | grep 220.181.38.183| awk '{print $1"/t"$8}' | sort | uniq -c | sort -nr | less</span>
  </p>
  
  <p style="text-align:left;line-height:30px">
    <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">7,</span><span style="font-size:14px;font-family:宋体;color:#222222">查看某一时间段的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">ip</span><span style="font-size:14px;font-family:宋体;color:#222222">连接数</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">:<br /> grep "2006:0[7-8]" www20110519.log | awk '{print $2}' | sort | uniq -c| sort -nr | wc -l</span>
  </p>
  
  <p style="text-align:left;line-height:30px">
    <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">8,</span><span style="font-size:14px;font-family:宋体;color:#222222">当前</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">WEB</span><span style="font-size:14px;font-family:宋体;color:#222222">服务器中联接次数最多的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">20</span><span style="font-size:14px;font-family:宋体;color:#222222">条</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">ip</span><span style="font-size:14px;font-family:宋体;color:#222222">地址</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">:<br /> netstat -ntu |awk '{print $5}' |sort | uniq -c| sort -n -r | head -n 20</span>
  </p>
  
  <p style="text-align:left;line-height:30px">
    <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">9,</span><span style="font-size:14px;font-family:宋体;color:#222222">查看日志中访问次数最多的前</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">10</span><span style="font-size:14px;font-family:宋体;color:#222222">个</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">IP<br /> cat access_log |cut -d ' ' -f 1 |sort |uniq -c | sort -nr | awk '{print $0 }' | head -n 10 |less</p> 
    
    <p>
      10,</span><span style="font-size:14px;font-family:宋体;color:#222222">查看日志中出现</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">100</span><span style="font-size:14px;font-family:宋体;color:#222222">次以上的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">IP<br /> cat access_log |cut -d ' ' -f 1 |sort |uniq -c | awk '{if ($1 > 100) print $0}'</span><span style="font-size:14px;font-family:宋体;color:#222222">｜</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">sort -nr |less</span>
    </p>
    
    <p style="text-align:left;line-height:30px">
      <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">11,</span><span style="font-size:14px;font-family:宋体;color:#222222">查看最近访问量最高的文件</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> cat access_log |tail -10000|awk '{print $7}'|sort|uniq -c|sort -nr|less</p> 
      
      <p>
        12,</span><span style="font-size:14px;font-family:宋体;color:#222222">查看日志中访问超过</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">100</span><span style="font-size:14px;font-family:宋体;color:#222222">次的页面</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> cat access_log | cut -d ' ' -f 7 | sort |uniq -c | awk '{if ($1 > 100) print $0}' | less</span>
      </p>
      
      <p style="text-align:left;line-height:30px">
        <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">13,</span><span style="font-size:14px;font-family:宋体;color:#222222">列出传输时间超过</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"> 30 </span><span style="font-size:14px;font-family:宋体;color:#222222">秒的文件</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> cat access_log|awk '($NF > 30){print $7}'|sort -n|uniq -c|sort -nr|head -20</span>
      </p>
      
      <p style="text-align:left;line-height:30px">
        <span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">14,</span><span style="font-size:14px;font-family:宋体;color:#222222">列出最最耗时的页面</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">(</span><span style="font-size:14px;font-family:宋体;color:#222222">超过</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">60</span><span style="font-size:14px;font-family:宋体;color:#222222">秒的</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222">)</span><span style="font-size:14px;font-family:宋体;color:#222222">的以及对应页面发生次数</span><span style="font-size:14px;font-family:&#39;Tahoma&#39;,sans-serif;color:#222222"><br /> cat access_log |awk '($NF > 60 && $7~//.php/){print $7}'|sort -n|uniq -c|sort -nr|head -100</span>
      </p>
      
      <p>
        &nbsp;
      </p>
      
      <p>
      </p>