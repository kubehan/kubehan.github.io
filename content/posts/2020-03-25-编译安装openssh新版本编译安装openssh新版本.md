---
title: 编译安装openssh新版本编译安装openssh新版本
author: Kubehan
type: post
date: 2020-03-25T10:00:06+08:00
url: /1901.html
views:
  - 1011
  - 1011
zm_like:
  - 2
  - 2
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/openssh.jpg
  - https://www.kubehan.cn/wp-content/uploads/2020/03/openssh.jpg
Baidusubmit:
  - 1
categories:
  - 基线检查
  - 等保测评

---
reference1: <https://blog.csdn.net/qq_33684555/article/details/81872110>

reference2: <http://www.cnblogs.com/xiaochina/p/7486073.html>

reference3: <https://blog.csdn.net/bytxl/article/details/46639073>

reference4: <https://www.cnblogs.com/xshrim/p/6472679.html> -> 含有zlib编译

reference5: <https://www.linuxidc.com/Linux/2011-10/45739p2.htm> -> 定义了sshd_config文件中的配置

OpenSSH（OpenBSD Secure Shell）是OpenBSD计划组所维护的一套用于安全访问远程计算机的连接工具。该工具是SSH协议的开源实现，支持对所有的传输进行加密，可有效阻止窃听、连接劫持以及其他网络级的攻击。  
OpenSSH中的scp client实用程序存在安全漏洞，该漏洞源于程序错误的验证了对象名称。攻击者可利用该漏洞覆盖文件。  
最近公司发现OPENSSH有防护漏洞, 需要给7.9版本的openssh安装补丁, 故网上收集资料, 融合自己的理解, 写出此文, 有任何问题请指正.

**注意: 编译安装openssh新版本之前, 最好先安装telnet服务, 万一sshd服务被你操作挂了, 还有替代的telnet服务可以提供远程连接, 笔者就是在安装过程中遇到过此问题, 最后拖了两天时间, 各种找领导, 然后登录堡垒机直接操作Centos虚拟机, 才解决问题!!!**

<pre><code class="shell">yum -y install telnet telnet-server
systemctl start telnet.socket
# 注: 使用telnet命令可直接远程连接服务器, 但是telnet不允许root用户登录!
</code></pre>

## 1. 基础环境准备

首先去openssh官网下载openssh.tar.gz最新版本

<pre><code class="shell">wget https://openbsd.hk/pub/OpenBSD/OpenSSH/portable/openssh-8.0p1.tar.gz
</code></pre>

**注意要下载p1版本, 此版为编译安装包!!**

在安装之前先记下sshd.pid路径, 因为在启动文件sshd中要更改此路径

将安装包先传入服务器中

卸载现有版本openssh: `rpm -e --nodeps $(rpm -qa | grep openssh)`

删除/etc/ssh/下所有文件, 在卸载完openssh后此路径下文件不会删除, 需手动删除

安装依赖：zlib-devel、openssl-devel、gcc、gcc-c++、make等, 可直接`yum groupinstall "Development Tools"`, 出现"No packages in any requested group available to install or update"报错时:

<pre><code class="shell">yum -y install openssl-devel
yum -y groupinstall "Development Tools"  
    --setopt=group_package_types=mandatory,default,optional

# 报错原因详见Redhat官网:
# https://access.redhat.com/solutions/1310043
</code></pre>

, 唯一需要注意的是openssl-devel这个包必须安装

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 277px;">
    <div>
    </div>
    
    <div class="image-view" data-width="900" data-height="277">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-156fc98aae35b5191d6944b55afacce9.png" alt="编译安装openssh新版本编译安装openssh新版本" />
    </div>
  </div>
  
  <div class="image-caption">
    编译安装openssh最新版本.png
  </div>
</div>

## 2. 编译安装openssh

<pre><code class="shell"># 先设置selinux为permissive
vi /etc/sysconfig/selinux

./configure --prefix=/usr/             
             --sysconfdir=/etc/ssh/        
             --with-ssl-dir=/usr/local/ssl 
             --with-md5-passwords          
             --mandir=/usr/share/man/

make && make install

# 删除/etc/ssh/下的密钥对, (只删除密钥对即可, 在重启的时候会重新生成), 此步省略!!!
rm -f /etc/ssh/ssh_host_*  

# 复制启动文件至/etc/init.d/
cp -a contrib/redhat/sshd.init /etc/init.d/sshd

# 编辑/etc/init.d/sshd文件, 将PID_FILE改为之前记下的sshd.pid路径
vi /etc/init.d/sshd

# 设置开机启动sshd
chkconfig sshd on
chkconfig --list sshd

# 编辑/etc/ssh/sshd_config, 设置一些常用参数
vi /etc/ssh/sshd_config
PermitRootLogin yes
PubkeyAuthentication yes
PasswordAuthentication yes
AllowTcpForwarding yes
X11Forwarding yes
PidFile /run/sshd.pid

# 编辑/usr/lib/systemd/system/sshd.service, 添加以下内容
vi /usr/lib/systemd/system/sshd.service
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
#After=network.target sshd-keygen.service
#Wants=sshd-keygen.service
After=network.target

[Service]
#Type=notify
#EnvironmentFile=/etc/sysconfig/sshd
#ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecStart=/usr/sbin/sshd
#ExecReload=/bin/kill -HUP $MAINPID
#KillMode=process
#Restart=on-failure
#RestartSec=42s

[Install]
WantedBy=multi-user.target

# 编辑好这个配置文件之后进行以下操作
systemctl enable sshd
systemctl status sshd
systemctl restart sshd
systemctl status sshd
</code></pre>

--prefix 安装目录  
--sysconfdir 配置文件目录  
--with-ssl-dir 指定 OpenSSL 的安装目录  
--with-privsep-path 非特权用户的chroot目录  
--with-privsep-user=sshd 指定非特权用户为sshd  
--with-zlib 指定zlib库的安装目录  
--with-md5-passwords 支持读取经过MD5加密的口令  
--with-ssl-engine 启用OpenSSL的ENGINE支持

<div class="image-package">
  <div class="image-container" style="max-width: 656px; max-height: 124px;">
    <div>
    </div>
    
    <div class="image-view" data-width="656" data-height="124">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-581da694502e31e3d5300edca016bc0d.png" alt="编译安装openssh新版本编译安装openssh新版本" />
    </div>
  </div>
  
  <div class="image-caption">
    编译安装openssh最新版本1.png
  </div>
</div>

至此openssh服务升级完成, 将sshd服务添加到开机启动即可

此版本openssh的配置文件默认是没有开启root登录权限的, 想要开启权限可以修改配置文件/etc/ssh/sshd\_config, 配置PermitRootLogin yes, 取消注释 PasswordAuthentication yes, 配置PID\_FILE=/run/sshd.pid

<div class="image-package">
  <div class="image-container" style="max-width: 295px; max-height: 104px;">
    <div>
    </div>
    
    <div class="image-view" data-width="295" data-height="104">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-fd5954a53b0123f78fc284d33dabd30d.png" alt="编译安装openssh新版本编译安装openssh新版本" />
    </div>
  </div>
  
  <div class="image-caption">
    编译安装openssh最新版本2.png
  </div>
</div>

另外, 安装目录为/usr/, 因为在启动文件中有关于SSHD的路径, 此安装目录默认为redhat启动文件的路径

<div class="image-package">
  <div class="image-container" style="max-width: 608px; max-height: 72px;">
    <div>
    </div>
    
    <div class="image-view" data-width="608" data-height="72">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-fb41a74eef72cba86e2b3ca156d51afe.png" alt="编译安装openssh新版本编译安装openssh新版本" />
    </div>
  </div>
  
  <div class="image-caption">
    编译安装openssh最新版本3.png
  </div>
</div>

如果变更了安装路径, 启动文件的此路径也要变更

## 3. 编译完成后可能遇到的问题

xshell等连接报错如下: The SSH server rejected your password. Try again.

<div class="image-package">
  <div class="image-container" style="max-width: 437px; max-height: 396px;">
    <div>
    </div>
    
    <div class="image-view" data-width="437" data-height="396">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-ad2e752410ce46fba2993a330ef79c77.png" alt="编译安装openssh新版本编译安装openssh新版本" />
    </div>
  </div>
  
  <div class="image-caption">
    编译安装openssh最新版本4.png
  </div>
</div>

或者用journalctl -xn查看后台日志时发现: error: Could not get shadow information for root

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 127px;">
    <div>
    </div>
    
    <div class="image-view" data-width="837" data-height="127">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-18c01e1abd35a79d7b07d45fd488417f.png" alt="编译安装openssh新版本编译安装openssh新版本" />
    </div>
  </div>
  
  <div class="image-caption">
    编译安装openssh最新版本5.png
  </div>
</div>

则是由于服务器上的**selinux**的问题, 需要将selinux改为permissive

<pre><code class="shell">在开启SSHD服务时报错.
sshd re-exec requires execution with an absolute path
用绝对路径启动,也报错如下:
Could not load host key: /etc/ssh/ssh_host_key
Could not load host key: /etc/ssh/ssh_host_rsa_key
Could not load host key: /etc/ssh/ssh_host_dsa_key
Disabling protocol version 1. Could not load host key
Disabling protocol version 2. Could not load host key
sshd: no hostkeys available — exiting
解决过程:
#ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
#ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
#/usr/sbin/sshd

如果上述两个文件存在，仍然出现这个错误，那么试试 chmod 600 上述两个文件。之后应该可以解决。
</code></pre>

## 4. 将sshd服务添加到开机启动

<pre><code class="shell">vi /usr/lib/systemd/system/sshd.service

[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
#After=network.target sshd-keygen.service
#Wants=sshd-keygen.service
After=network.target

[Service]
#Type=notify
#EnvironmentFile=/etc/sysconfig/sshd
#ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecStart=/usr/sbin/sshd
#ExecReload=/bin/kill -HUP $MAINPID
#KillMode=process
#Restart=on-failure
#RestartSec=42s

[Install]
WantedBy=multi-user.target
</code></pre>

<pre><code class="shell">vi sshd@.service # 可省略

[Unit]
Description=OpenSSH per-connection server daemon
Documentation=man:sshd(8) man:sshd_config(5)
Wants=sshd-keygen.service
After=sshd-keygen.service

[Service]
EnvironmentFile=-/etc/sysconfig/sshd
ExecStart=-/usr/sbin/sshd -i $OPTIONS
StandardInput=socket
</code></pre>

<pre><code class="shell">vi sshd.socket # 可省略

[Unit]
Description=OpenSSH Server Socket
Documentation=man:sshd(8) man:sshd_config(5)
Conflicts=sshd.service

[Socket]
ListenStream=22
Accept=yes

[Install]
WantedBy=sockets.target
</code></pre>

<pre><code class="shell">vi sshd-keygen.service # 可省略

[Unit]
Description=OpenSSH Server Key Generation
ConditionFileNotEmpty=|!/etc/ssh/ssh_host_rsa_key
ConditionFileNotEmpty=|!/etc/ssh/ssh_host_ecdsa_key
ConditionFileNotEmpty=|!/etc/ssh/ssh_host_ed25519_key
PartOf=sshd.service sshd.socket

[Service]
ExecStart=/usr/sbin/sshd-keygen
Type=oneshot
RemainAfterExit=yes
</code></pre>

<pre><code class="shell">vi /usr/lib/systemd/system/sshd.service # 编辑好这个配置文件之后进行以下操作
systemctl enable sshd
systemctl status sshd
systemctl restart sshd
systemctl status sshd
</code></pre>