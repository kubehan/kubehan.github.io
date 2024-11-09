---
title: Windows server安装mysql8.0
author: Kubehan
type: post
date: 2020-05-14T07:08:06+08:00
url: /2586.html
Baidusubmit:
  - 1
views:
  - 902
baidu_record:
  - 1
zm_like:
  - 1
categories:
  - Mysql

---
# Windows server安装mysql8.0

这里不像网上那么繁琐了，你既然知道安装mysql，想必您也具有一点点点点的mysql基础,这就足够了，

1.首先下载mysql的二进制包： [点击下载mysql8.0.13][1] 

2.将包解压到你想要用了作为mysql的安装路径的地方

3.管理员运行cmd

4.cd 解压路径/包名/bin 目录下，为了执行mysql的部分命令

5.进行数据库初始化： 

<pre><code class="language-sql">mysqld --initialize --console </code></pre>

6.这里如果你没有安装 VC的话会报错

安装VC2015-2019补丁：[最新支持的 Visual C++ 下载][2]

安装简单，下载下来一步步运行就行了

7.在解压目录下执行 mysqld --initialize 初始化数据库，如果一开始就存在这个data文件夹请先删除它 

8.到这里初始化就能成功了，

9.为了方便管理，可添加配置文件my.ini 要特别注意文件的编码，我在另一篇文章有记录这个问题

10.安装mysql为服务

<pre><code class="language-sql">mysqld --install</code></pre>

11.启动mysql服务 

<pre><code class="language-powershell">net start mysql </code></pre>

12.用初始化时记录的密码登录数据库修改密码

<pre><code class="language-sql">mysql -uroot -ppassword #登录
use mysql; #选择数据库
ALTER USER &#039;root&#039;@&#039;localhost&#039; IDENTIFIED BY &#039;password&#039; PASSWORD EXPIRE NEVER; #更改加密方式
FLUSH PRIVILEGES; #刷新权限</code></pre>

以上所述是小编给大家介绍的解决Windows安装MySQL8.0 步骤及出现错误问题,希望对大家有所帮助，如果大家有任何疑问请给我留言，小编会及时回复大家的。在此也非常感谢大家的支持！  
如果你觉得本文对你有帮助，欢迎转载，烦请注明出处，谢谢！

 [1]: https://dev.mysql.com/downloads/file/?id=476233 "点击下载mysql8.0.13"
 [2]: https://support.microsoft.com/zh-cn/help/2977003/the-latest-supported-visual-c-downloads "最新支持的 Visual C++ 下载"