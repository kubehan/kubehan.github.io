---
title: 重要通知｜JumpServer漏洞通知及修复方案
author: Kubehan
type: post
date: 2021-01-16T02:19:29+08:00
url: /3171.html
views:
  - 751

---
<p style='max-width: 100%;min-height: 1em;font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;background-color: rgb(255, 255, 255);text-align: center;box-sizing: border-box !important;overflow-wrap: break-word !important;' data-mpa-powered-by="yiban.io">
  <span style='max-width: 100%;font-size: 13px;color: rgb(136, 136, 136);font-family: Georgia, "Times New Roman", Times, "Songti SC", serif;letter-spacing: 0.544px;box-sizing: border-box !important;overflow-wrap: break-word !important;'>点击上方“</span><span style='max-width: 100%;font-size: 13px;font-family: Georgia, "Times New Roman", Times, "Songti SC", serif;letter-spacing: 0.544px;box-sizing: border-box !important;overflow-wrap: break-word !important;'><span style="color:#0080ff;">民工哥技术之路</span></span><span style='max-width: 100%;font-size: 13px;color: rgb(136, 136, 136);font-family: Georgia, "Times New Roman", Times, "Songti SC", serif;letter-spacing: 0.544px;box-sizing: border-box !important;overflow-wrap: break-word !important;'>”，选择“设为星标”</span><strong style="max-width: 100%;letter-spacing: 0.544px;color: rgb(74, 74, 74);font-family: Avenir, -apple-system-font, 微软雅黑, sans-serif;font-size: 16px;white-space: pre-line;text-align: right;box-sizing: border-box !important;overflow-wrap: break-word !important;"></strong>
</p>

<p style='max-width: 100%;min-height: 1em;font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;background-color: rgb(255, 255, 255);text-align: center;box-sizing: border-box !important;overflow-wrap: break-word !important;'>
  <span style="max-width: 100%;font-size: 13px;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span style='max-width: 100%;color: rgb(136, 136, 136);font-family: Georgia, "Times New Roman", Times, "Songti SC", serif;letter-spacing: 0.544px;box-sizing: border-box !important;overflow-wrap: break-word !important;'>回复“</span><span style='max-width: 100%;font-family: Georgia, "Times New Roman", Times, "Songti SC", serif;letter-spacing: 0.544px;box-sizing: border-box !important;overflow-wrap: break-word !important;'><span style="color:#0080ff;">1024</span></span><span style='max-width: 100%;color: rgb(136, 136, 136);font-family: Georgia, "Times New Roman", Times, "Songti SC", serif;letter-spacing: 0.544px;box-sizing: border-box !important;overflow-wrap: break-word !important;'>”获取独家整理的学习资料！</span></span>
</p>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/01/frc-038a6f8a5ab1b30e1bd5591658d8d810.jpeg" /> <section data-tool="mdnice编辑器" data-website="https://www.mdnice.com" style='line-height: 1.6;word-break: break-word;text-align: left;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif;padding: 5px;font-size: 16px;color: rgb(53, 53, 53);word-spacing: 0.8px;letter-spacing: 0.8px;border-radius: 16px;'> 

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  2021年1月15日，JumpServer开源堡垒机发现一处远程执行漏洞，需要用户尽快进行修复，尤其是可通过公网访问的JumpServer堡垒机用户建议尽快进行修复。
</p>

<h2 data-tool="mdnice编辑器" style="font-weight: bold;color: black;font-size: 22px;margin-top: 20px;margin-right: 10px;">
  <span style="display: none;"></span><span style="font-size: 18px;color: rgb(34, 34, 34);display: inline-block;padding-left: 10px;border-left: 5px solid rgb(248, 57, 41);">影响版本如下：</span><br />
</h2>

<ul data-tool="mdnice编辑器" style="margin-top: 8px;margin-bottom: 8px;padding-left: 25px;color: rgb(248, 57, 41);" class="list-paddingleft-2">
  <li>
    <section style="margin-top: 5px;margin-bottom: 5px;line-height: 26px;color: rgb(53, 53, 53);">JumpServer堡垒机＜v2.6.2版本</section>
  </li>
  <li>
    <section style="margin-top: 5px;margin-bottom: 5px;line-height: 26px;color: rgb(53, 53, 53);">JumpServer堡垒机＜v2.5.4版本</section>
  </li>
  <li>
    <section style="margin-top: 5px;margin-bottom: 5px;line-height: 26px;color: rgb(53, 53, 53);">JumpServer堡垒机＜v2.4.5版本</section>
  </li>
</ul>

<h2 data-tool="mdnice编辑器" style="font-weight: bold;color: black;font-size: 22px;margin-top: 20px;margin-right: 10px;">
  <span style="display: none;"></span><span style="font-size: 18px;color: rgb(34, 34, 34);display: inline-block;padding-left: 10px;border-left: 5px solid rgb(248, 57, 41);">安全版本如下：</span><br />
</h2>

<ul data-tool="mdnice编辑器" style="margin-top: 8px;margin-bottom: 8px;padding-left: 25px;color: rgb(248, 57, 41);" class="list-paddingleft-2">
  <li>
    <section style="margin-top: 5px;margin-bottom: 5px;line-height: 26px;color: rgb(53, 53, 53);">JumpServer堡垒机＞=v2.6.2版本</section>
  </li>
  <li>
    <section style="margin-top: 5px;margin-bottom: 5px;line-height: 26px;color: rgb(53, 53, 53);">JumpServer堡垒机＞=v2.5.4版本</section>
  </li>
  <li>
    <section style="margin-top: 5px;margin-bottom: 5px;line-height: 26px;color: rgb(53, 53, 53);">JumpServer堡垒机＞=v2.4.5版本</section>
  </li>
</ul>

<h2 data-tool="mdnice编辑器" style="font-weight: bold;color: black;font-size: 22px;margin-top: 20px;margin-right: 10px;">
  <span style="display: none;"></span><span style="font-size: 18px;color: rgb(34, 34, 34);display: inline-block;padding-left: 10px;border-left: 5px solid rgb(248, 57, 41);">修复方案</span><br />
</h2>

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  建议JumpServer堡垒机（含社区版及企业版）用户升级至安全版本。
</p>

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  <span style="font-weight: 700;color: rgb(248, 57, 41);">临时修复方案</span>
</p>

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  修改Nginx配置文件，以屏蔽漏洞接口 ：
</p>

<pre data-tool="mdnice编辑器" style="margin-top: 10px;margin-bottom: 10px;"><code style="overflow-x: auto;padding: 16px;background: #1E1E1E;color: #DCDCDC;display: -webkit-box;font-family: Operator Mono, Consolas, Monaco, Menlo, monospace;border-radius: 0px;font-size: 12px;-webkit-overflow-scrolling: touch;">/api/v1/authentication/connection-token/&lt;br>/api/v1/users/connection-token/&lt;br></code></pre>

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  Nginx配置文件位置如下：
</p>

<pre data-tool="mdnice编辑器" style="margin-top: 10px;margin-bottom: 10px;"><code style="overflow-x: auto;padding: 16px;background: #1E1E1E;color: #DCDCDC;display: -webkit-box;font-family: Operator Mono, Consolas, Monaco, Menlo, monospace;border-radius: 0px;font-size: 12px;-webkit-overflow-scrolling: touch;">社区老版本&lt;br>/etc/nginx/conf.d/jumpserver.conf&lt;br>&lt;span style="color: #57A64A;font-style: italic;line-height: 26px;"># 企业老版本&lt;/span>&lt;br>jumpserver-release/nginx/http_server.conf&lt;br>&lt;span style="color: #57A64A;font-style: italic;line-height: 26px;"># 新版本在 &lt;/span>&lt;br>jumpserver-release/compose/config_static/http_server.conf&lt;br></code></pre>

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  Nginx配置文件实例为：
</p>

<pre data-tool="mdnice编辑器" style="margin-top: 10px;margin-bottom: 10px;"><code style="overflow-x: auto;padding: 16px;background: #1E1E1E;color: #DCDCDC;display: -webkit-box;font-family: Operator Mono, Consolas, Monaco, Menlo, monospace;border-radius: 0px;font-size: 12px;-webkit-overflow-scrolling: touch;">&lt;span style="color: #57A64A;font-style: italic;line-height: 26px;">#保证在 /api 之前 和 / 之前&lt;/span>&lt;br>location /api/v1/authentication/connection-token/ {&lt;br>   &lt;span style="color: #4EC9B0;line-height: 26px;">return&lt;/span> 403;&lt;br>}&lt;br> &lt;br>location /api/v1/users/connection-token/ {&lt;br>   &lt;span style="color: #4EC9B0;line-height: 26px;">return&lt;/span> 403;&lt;br>}&lt;br>&lt;span style="color: #57A64A;font-style: italic;line-height: 26px;">#新增以上这些&lt;/span>&lt;br> &lt;br>location /api/ {&lt;br>    proxy_set_header X-Real-IP &lt;span style="color: #BD63C5;line-height: 26px;">$remote_addr&lt;/span>;&lt;br>    proxy_set_header Host &lt;span style="color: #BD63C5;line-height: 26px;">$host&lt;/span>;&lt;br>    proxy_set_header X-Forwarded-For &lt;span style="color: #BD63C5;line-height: 26px;">$proxy_add_x_forwarded_for&lt;/span>;&lt;br>    proxy_pass http://core:8080;&lt;br>  } &lt;br>...&lt;br></code></pre>

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  修改配置文件完毕后，重启Nginx服务即可。
</p>

<p data-tool="mdnice编辑器" style="padding-top: 8px;padding-bottom: 8px;line-height: 1.75;margin-top: 0.8em;margin-bottom: 0.8em;">
  来源：JumpServer开源堡垒机 https://blog.fit2cloud.com/?p=1761
</p></section> <section data-mpa-template="t" mpa-from-tpl="t"> <section data-id="89429" mpa-from-tpl="t"> <section> <section mpa-from-tpl="t"> 

<p style="text-align: center;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/01/frc-41a69ef07b1f6f2e90d6cece54681cd0.jpeg" />
</p>

<p style="text-align: center;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/01/frc-9d4e27e080ab785f6dc13756de5780f0.jpeg" />
</p>

<p style="text-align: center;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/01/frc-c6499bbecc67696e6d33ed79754ecc8c.jpeg" />
</p>

<p style="text-align: center;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/01/frc-78ed1da8bca7e7584fcae75e92360b73.jpeg" />
</p></section> </section> </section> </section> <section style='max-width: 100%;min-height: 1em;white-space: normal;font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;background-color: rgb(255, 255, 255);color: rgb(0, 0, 0);font-size: medium;letter-spacing: 1px;margin-bottom: 15px;margin-top: 15px;overflow-wrap: break-word !important;box-sizing: border-box !important;'>

<span style="max-width: 100%;font-size: 13px;overflow-wrap: break-word !important;box-sizing: border-box !important;"><span style="max-width: 100%;overflow-wrap: break-word !important;box-sizing: border-box !important;"><span style="padding: 3px 8px;max-width: 100%;font-family: inherit;text-align: inherit;text-decoration: inherit;letter-spacing: 0.544px;caret-color: rgb(0, 0, 0);border-radius: 4px;color: rgb(255, 255, 255);background-color: rgb(255, 105, 31);font-size: 16px;box-sizing: border-box !important;overflow-wrap: break-word !important;">推荐阅读</span><span style='max-width: 100%;color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;font-size: 13px;letter-spacing: 0.544px;text-align: start;caret-color: rgb(0, 0, 0);background-color: rgb(255, 255, 255);box-sizing: border-box !important;overflow-wrap: break-word !important;'> </span><span style="margin-left: 4px;padding: 3px 8px;max-width: 100%;font-family: inherit;text-align: inherit;text-decoration: inherit;letter-spacing: 0.544px;caret-color: rgb(0, 0, 0);border-radius: 1.2em;color: rgb(255, 255, 255);line-height: 1.2;background-color: rgb(204, 204, 204);border-color: rgb(249, 110, 87);font-size: 12px;box-sizing: border-box !important;overflow-wrap: break-word !important;">点击标题可跳转</span></span></span></section> 

<p style="margin-top: 5px;white-space: normal;">
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506691&idx=1&sn=ad34b0a25f3c195cb67a85bf5774e030&chksm=e918be1fde6f37090713d25359ed761c3372c488d2e49e13c8d25fe040070919c631d40ec612&scene=21#wechat_redirect" textvalue="996 违法？？？" data-itemshowtype="0" tab="innerlink" style="text-decoration: underline;font-size: 13px;" data-linktype="2" rel="noopener noreferrer">996 违法？？？真相是这样的。。</a>
</p>

<p style="margin-top: 5px;white-space: normal;">
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506647&idx=1&sn=3283c10df2814a41e23b3e130d30db6c&chksm=e918bfcbde6f36dd274ba8179dbde9c7c948ebb3c7e8deebdaedc9ce708b7c97ab19df261aff&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" style="text-decoration: underline;font-size: 13px;" data-linktype="2" rel="noopener noreferrer">微信出硬件了！或于春节上线</a>
</p>

<p style="margin-top: 5px;white-space: normal;">
  <span style="font-size: 13px;text-decoration: underline;"><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506595&idx=1&sn=2baa81b2f91f4c48885200341b93fcc9&chksm=e918bfbfde6f36a9e3b236eca2ec68c9ae9278daa518f9c8f2098fb4f1f79317ebc925748120&scene=21#wechat_redirect" textvalue="淦！又是美团。。。。" data-itemshowtype="0" tab="innerlink" data-linktype="2" rel="noopener noreferrer">淦！又是美团。。。。这次吃相很难看</a>！</span>
</p>

<p style="margin-top: 5px;white-space: normal;">
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506525&idx=1&sn=f62a2391b5b9337ca7d4f3ab6873b0a1&chksm=e918bf41de6f3657214b067544f6bf65de6e6cfe704cf993ac4298167a1fe34111b7304ec52b&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" style="text-decoration: underline;font-size: 13px;" data-linktype="2" rel="noopener noreferrer">全球最大色情网站宣布：封杀特朗普</a>
</p>

<p style="margin-top: 5px;white-space: normal;">
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506441&idx=2&sn=eba20b14de948226c200dc93c974656b&chksm=e918bf15de6f3603bb81cf0d5c8a4a23bf2be1d880fa4502c57f9c0d78264d21d17ce966996c&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" style="text-decoration: underline;font-size: 13px;" data-linktype="2" rel="noopener noreferrer">分布式存储 GlusterFS 介绍与部署</a>
</p>

<p style="margin-top: 5px;white-space: normal;">
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506354&idx=1&sn=6f8d24fd517ab236c6609ffcc861c4b6&chksm=e918bcaede6f35b8b0bae415d0fa3d7bedbc2ce338803751759403657d0ea09c833bedad3f8f&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" style="text-decoration: underline;font-size: 13px;" data-linktype="2" rel="noopener noreferrer">红旗 Linux 桌面操作系统 11 来了</a>
</p>

<p style="margin-top: 5px;white-space: normal;">
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506691&idx=2&sn=45645ef63701a94ec5d71273322a75b9&chksm=e918be1fde6f3709eaa33d9706c1e803b7816e3158f5c642fe949230094e4142d4dbc6528d0a&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" style="text-decoration: underline;font-size: 13px;" data-linktype="2" rel="noopener noreferrer">Redis 6.0 集群搭建实践</a>
</p>

<p style="margin-top: 5px;white-space: normal;">
  <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI0MDQ4MTM5NQ==&mid=2247506289&idx=1&sn=87abe16e16d0a7c2478c0aa3b60cfeef&chksm=e918bc6dde6f357bcc0e8b5d09a00f42b24425553b527c5e9bc149a109fcb7a0a23b869fb2fa&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" style="text-decoration: underline;font-size: 13px;" data-linktype="2" rel="noopener noreferrer">华为悄悄推出"应用市场"，免费、无广告，贼好用！</a><strong style='color: rgb(255, 76, 0);background-color: rgb(255, 255, 255);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;font-size: 16px;letter-spacing: 0.544px;text-align: center;'><span style="font-size: 14px;"></span></strong>
</p><section style="text-align: center;margin-top: 10px;">

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/01/frc-b3b4afa757b0351963352d38de8ae2b7.png" /> </section> <section style="max-width: 100%;overflow-wrap: break-word !important;box-sizing: border-box !important;"> <section data-role="paragraph" style="max-width: 100%;overflow-wrap: break-word !important;box-sizing: border-box !important;"> <section> 

<p style="text-align: center;">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2021/01/frc-152277e017c4a86ef69331d2ebfc72d4.gif" />
</p></section> </section> </section>