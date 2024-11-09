---
title: Docker镜像之开源镜像加速--切换docker镜像源
author: Kubehan
type: post
date: 2020-04-09T07:25:32+08:00
url: /2191.html
classic-editor-remember:
  - classic-editor
  - classic-editor
Baidusubmit:
  - 1
  - 1
views:
  - 2647
  - 2647
zm_like:
  - 1
categories:
  - Devops
  - Docker

---
大家都知道，安装使用docker的时候默认的是国外的镜像源，使用起来有点难度，这里说说怎么切换docker源为国内的镜像源

国内下载docker镜像大部分都比较慢，下面给大家介绍2个镜像源

#### 阿里云的镜像源

注册阿里云用户获取镜像源：<a href="https://cr.console.aliyun.com/#/accelerator" target="_blank" rel="noopener noreferrer">点击跳转获取加速地址</a>

地址填写自己的专属加速地址

使用的时候修改/etc/docker/daemon.json文件就可以了，

<pre class="brush:bash;toolbar:false">cat&nbsp;/etc/docker/daemon.json
{
&nbsp;&nbsp;"registry-mirrors":&nbsp;["https://xk4sok19.mirror.aliyuncs.com"]
}</pre>

修改保存后重启 Docker 以使配置生效

#### docker中国官方的镜像源

<pre class="brush:bash;toolbar:false">cat&nbsp;/etc/docker/daemon.json
{
&nbsp;&nbsp;"registry-mirrors":&nbsp;["https://registry.docker-cn.com"]
}</pre>

<span style="font-size: 14px;">修改保存后重启 Docker 以使配置生效</span>