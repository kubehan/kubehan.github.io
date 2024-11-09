---
title: 记录一次confluence升级处理过程
author: Kubehan
type: post
date: 2020-08-04T03:49:40+08:00
url: /2665.html
Baidusubmit:
  - 1
views:
  - 2424
zm_like:
  - 1
categories:
  - Linux运维

---
# 记录一次confluence升级处理过程

仅记录我遇到的问题，如有相似的可以参考我的处理办法

## 整体升级步骤

1.备份原来的confluence数据

<pre><code class="language-shell">mv /var/atlassian/  /var/atlassian_backup/
mv /opt/atlassian /opt/atlassian_backup/
mv /etc/init.d/confluence /etc/init.d/confluence_bak
及备份数据库mysqldump</code></pre>

2.安装新版的confluence，我这里是7.4.3

3.安装完后将备份的数据库导入新数据库（如果数据也升级的话）

4.登录到confluence的备份还原里面按提示操作

<pre><code class="language-shell">#把备份复制到恢复目录
cp /var/atlassian_backup/application-data/confluence/backups/ /var/atlassian/application-data/confluence/restore/</code></pre>

5.重启confluence就能完成升级

大体的步骤就是上面的了，下面是我在过程中遇到的一些问题记录及解决办法

<pre><code class="language-shell">1.Confluence Does Not Start with &#039;Detected tables with non-default character encoding/collation&#039; Message</code></pre>

这是由于你的数据字符集没有按照官方的提示进行创建，不符合要求

[参考文档：][1]

需要将数据库字设置成：utf8 -- UTF-8 Unicode utf8_bin

<pre><code class="language-shell">2.This page is for Confluence administrators. If you&#039;re seeing this page, your Confluence administrato</code></pre>

原因有很多种

[参考文档：][2]

我这里原因是confluence进程没有彻底关闭导致，按官方文档解决

<pre><code class="language-shell">3.Confluence will not start up because the build number in the Home Directory doesn&#039;t match the build number in the Database, after upgrade</code></pre>

由于升级后主目录中的内部版本号与数据库中的内部版本号不匹配，因此融合将无法启动

原因是由于导入旧版数据库，数据库里面的版本标识与配置文件的版本标识不一样导致，

[参考文档：][3]

解决办法：找到配置文件查看版本信息

  1. 通过运行**数据库**：
    
    <pre><code class="language-shell">从数据库的CONFVERSION表中查看buildNumber</code></pre>

  2. 通过在<confluence\_home\_directory> /confluence.cfg.xml文件中查找**主目录**：

<pre><code class="language-xml">&lt;confluence-configuration&gt;
&lt;buildNumber&gt; 4215 &lt;/ buildNumber&gt;
&lt;/ confluence-configuration&gt;</code></pre>

保证该数字和数据库里面的一致

**注意：启动confluence时最好是用自己专有的用户来启动，以免遇到因权限问题导致的启动失败**

 [1]: https://confluence.atlassian.com/confkb/confluence-does-not-start-with-detected-tables-with-non-default-character-encoding-collation-message-392888396.html
 [2]: https://confluence.atlassian.com/confkb/cluster-panic-due-to-multiple-deployments-201852123.html?_ga=2.64720618.430748327.1596506377-1617599542.1596188993
 [3]: https://confluence.atlassian.com/confkb/confluence-will-not-start-up-because-the-build-number-in-the-home-directory-doesn-t-match-the-build-number-in-the-database-after-upgrade-376834096.html