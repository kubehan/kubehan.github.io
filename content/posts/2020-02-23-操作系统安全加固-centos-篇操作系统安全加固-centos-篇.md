---
title: 操作系统安全加固 CentOS 篇操作系统安全加固 CentOS 篇
author: Kubehan
type: post
date: 2020-02-23T09:05:56+08:00
url: /352.html
wb_dl_type:
  - 0
  - 0
views:
  - 837
  - 837
post_views_count:
  - 0
  - 0
categories:
  - Linux运维

---
<div class="image-package">
  <div class="image-container" style="max-width: 512px; max-height: 512px;">
    <div>
    </div>
    
    <div class="image-view" data-width="512" data-height="512">
    </div>
  </div>
  
  <div class="image-caption">
  </div>
</div>

### 前言

随着移动互联网爆发及越来越广泛的应用，一个应用系统经常会面临内部和外部威胁的风险，互联网安全已经成为影响互联网应用的关键问题。系统外围一般都会有防火墙、IDS、IPS 、抗 DDOS 等一系列安全防护手段，在云平台同样提供云防火墙、WAF、漏扫等一系列手段进行安全防护。

但在应用系统上的安全漏洞并不能完全消除，如承载应用系统的网络设备、操作系统、数据库及应用系统本身可能均存在大量的安全漏洞；甚至承载云平台的 SDN（软件定义网络）、SDS（软件定义存储）及其相关联的服务的操作系统，也会存在安装配置不符合安全需求、安全漏洞没及时修补、参数配置错误、使用维护不符合安全需求、开放不必要的端口及服务等等。这些漏洞有可能被控制，会导致数据被篡改或窃取，甚至会毁坏应用系统，导致不可挽救的损失。

本篇文章旨在指导系统管理人员或安全检查人员进行 CentOS 6.4 操作系统的安全合规性检查和配置。

> 注： CentOS 版本不一致时，请使用不同版本的命令行！ 

### 简洁安装操作系统

使用最简洁的方式安装 CentOS ，仅安装操作系统启动必需的服务和软件，降低因软件包而导致出现安全漏洞的可能性。

### SSHbanner 信息

  1. 执行如下命令创建 ssh banner 信息文件：

<pre><code class="bash"># touch /etc/sshbanner
# chown bin:bin /etc/sshbanner
# chmod 644 /etc/sshbanner
# echo " Authorized users only. "   &gt;/etc/sshbanner
</code></pre>

> 可根据需要修改第 4 条命令中的 “警告文本”。 

<ol start="2">
  <li>
    修改 /etc/ssh/sshd_config 文件，添加如下行：
  </li>
</ol>

<pre><code class="bash"># vi /etc/ssh/sshd_config
  Banner /etc/sshbanner
</code></pre>

> 不同版本和编译安装的 SSH 服务配置文件位置不同 

<ol start="3">
  <li>
    重启SSH服务
  </li>
</ol>

<pre><code class="bash"># pkill sshd
# /usr/local/sbin/sshd
</code></pre>

> 由于执行下面操作会断开SSH服务，故建议所有安全加固做完后重启服务器，使该配置生效 

### 登录Banner提示

<pre><code class="bash">echo " Authorized users only. All activity may be monitored and reported " &gt; /etc/motd
</code></pre>

> 可根据需要命令中的 “警告文本”。 

### 删除root以外UID为0的用户

<pre><code class="bash">&lt;code># awk -F “：” &#039;($3==0)  {print  $1} &#039; /etc/passwd</code>
 # userdel "username"
&lt;/code></pre>

若有 UID 为 0 的其它用户，使用 userdel 删除用户。

### 防 syn 攻击优化检查主机访问控制

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/hosts.allow /etc/hosts.allow_bak
# cp -p /etc/hosts.deny /etc/hosts.deny_bak
</code></pre>

<ol start="2">
  <li>
    编辑 /etc/hosts.allow 和 /etc/hosts.deny ，文件的配置格式为：<code>Service:host [or network/netmask]</code>
  </li>
</ol>

<pre><code class="bash"># vi /etc/hosts.allow
 all:192.168.4.44:allow  // 允许单个IP访问所有服务进程
 sshd:192.168.1.:allow   // 允许192.168.1的整个网段访问SSH服务进程
</code></pre>

<pre><code class="bash">#vi /etc/hosts.deny
 sshd:all:deny`          // 拒绝其它所有 IP 访问 SSH 服务
</code></pre>

### 预防Flood攻击

<pre><code class="bash"># vim  /etc/sysctl.conf
  net.ipv4.tcp_syncookies = 1
# sysctl  -p               // 使之生效
</code></pre>

### 关闭不必要服务

  1. 关闭开启命令

<pre><code class="bash"># chkconfig 服务名 off/on     // 开启关闭开机启动项
# service 服务名 stop/start   // 天启关闭服务
</code></pre>

<ol start="2">
  <li>
    建议关闭的服务
  </li>
</ol>

`cups、portmap、nfslock、nfs、rpcbind、NetworkManager、iptables、ip6tables、netfs、qpidd、yum-updatesd、sendmail、postfix、rpcbind、bluetooth、firstboot、isdn、kudzu、smartd、autofs`

<ol start="3">
  <li>
    正常情况关闭完服务，使用 <code>netstat -lantup</code> 命令仅可看到 <code>sshd</code> 进程和业务进程。
  </li>
</ol>

### 禁止 ctrl+alt+del

<pre><code class="bash"># vi /etc/inittab
 ca::ctrlaltdel:/bin/true
</code></pre>

将行`ca::ctrlaltdel:/sbin/shutdown -r -t 4 now` 修改为 `ca::ctrlaltdel:/bin/true`

或者将此行删除、注释，修改为其它命令均可，如：

`ca::ctrlaltdel:/bin/echo ""CTRL-ALT-DEL is disabled""`

### 无用账户删除或锁定

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/passwd /etc/passwd.bak
# cp -p /etc/shadow /etc/shadow.bak
</code></pre>

<ol start="2">
  <li>
    锁定无用帐户：
  </li>
</ol>

**方法一：**在需要锁定的用户名的密码字段前面加 !! ，建议锁定的用户：lp、uucp、games、nobody 。

<pre><code class="bash"># vi /etc/shadow
test:!!$1$QD1ju03H$LbV4vdBbpw.MY0hZ2D/Im1:14805:0:99999:7:::
</code></pre>

**方法二：**

<pre><code class="bash"># passwd -l test         // 假设 test 为无用帐号
</code></pre>

**方法三：**将 `/etc/passwd` 文件中的无用帐户的 `shell` 域设置成 `/bin/false` 。

<pre><code class="bash"># vi /etc/passwd
 lp:x:4:7:lp:/var/spool/lpd:/bin/false
</code></pre>

### 设置登录超时

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/profile /etc/profile.bak
# cp -p /etc/csh.cshrc /etc/csh.cshrc.bak
</code></pre>

<ol start="2">
  <li>
    在 /etc/profile 文件增加以下两行：
  </li>
</ol>

<pre><code class="bash"># vi /etc/profile
 TMOUT=180
 export TMOUT
</code></pre>

<ol start="3">
  <li>
    修改 /etc/csh.cshrc 文件，添加如下行：
  </li>
</ol>

<pre><code class="bash"># vi /etc/csh.cshrc
set autologout=30
</code></pre>

改变这项设置后，重新登录才能有效。

### 限制 root 远程登录

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/securetty /etc/securetty_bak
# cp -p /usr/local/etc/sshd_config  /usr/local/etc/sshd_config_bak
</code></pre>

<ol start="2">
  <li>
    新建一个普通用户并设置高强度密码：
  </li>
</ol>

<pre><code class="bash"># useradd username
# passwd username
</code></pre>

<ol start="3">
  <li>
    禁止 root 用户远程登录系统：
  </li>
</ol>

<pre><code class="bash"># vi /etc/securetty
</code></pre>

注释掉 `pts/x` 的行，保存退出，则禁止了 root 从 telnet 登录。

<pre><code class="bash"># vi /usr/local/etc/sshd_config
</code></pre>

将 `PermitRootLogin`设置为 `no` 并不被注释，保存退出，则禁止了 root 从 ssh 登录。

### 用户缺省UMASK设置

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/profile /etc/profile_bak
# cp -p /etc/csh.login /etc/csh.login_bak
# cp -p /etc/csh.cshrc /etc/csh.cshrc_bak
# cp -p /etc/bashrc /etc/bashrc_bak
# cp -p /root/.bashrc /root/.bashrc_bak
# cp –p /root/.cshrc /root/.cshrc_bak
</code></pre>

<ol start="2">
  <li>
    修改 umask 设置：
  </li>
</ol>

<pre><code class="bash"># vi /etc/profile
# vi /etc/csh.login
# vi /etc/csh.cshrc
# vi /etc/bashrc
# vi /root/.bashrc
# vi /root/.cshrc

  umask 027
</code></pre>

将 umask 值修改为 027，保存退出。

<ol start="3">
  <li>
    配置文件中若没有，添加行<code>umask 027</code> 到配置文件。
  </li>
</ol>

### 密码复杂度要求

使用 `pam pam_cracklib module` 或 `pam_passwdqc module` 实现密码复杂度，两者不能同时使用。

**pam_cracklib 主要参数说明:**

| 参数         | 说明                                     |
| ---------- | -------------------------------------- |
| tretry=N   | 重试多少次后返回密码修改错误                         |
| difok=N    | 新密码必需与旧密码不同的位数                         |
| dcredit=N  | N >= 0密码中最多有多少个数字；N < 0密码中最少有多少个数字     |
| lcredit=N  | N >= 0密码中最多有多少个小写字母；N < 0密码中最少有多少个小写字母 |
| ucredit=N  | N >= 0密码中最多有多少个大写字母；N < 0密码中最少有多少个大写字母 |
| credit=N   | N >= 0密码中最多有多少个特殊字符；N < 0密码中最少有多少个特殊字符 |
| minclass=N | 密码组成(大/小字母，数字，特殊字符)                    |
| minlen=N   | 新密码最短长度                                |

**pam_passwdqc主要参数说明:**

| 参数         | 说明                                                                    |
| ---------- | --------------------------------------------------------------------- |
| mix        | 设置口令字最小长度，默认值是 mix=disabled。                                          |
| max        | 设置口令字的最大长度，默认值是 max=40。                                               |
| passphrase | 设置口令短语中单词的最少个数，默认值是 3 ，如果为 0 则禁用口令短语。                                 |
| match      | 设置密码串的常见程序，默认值是match=4。                                               |
| similar    | 设置当我们重设口令时，重新设置的新口令能否与旧口令相似。允许相似：similar=permit ；不允许相似：similar=deny 。 |
| random     | 设置随机生成口令字的默认长度。默认值是 42 ；设为 0 则禁止该功能。                                  |

**具体操作如下：**

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/pam.d/system-auth /etc/pam.d/system-auth_bak
# cp -p /etc/pam.d/passwd /etc/pam.d/passwd_bak
</code></pre>

<ol start="2">
  <li>
    修改策略设置：
  </li>
</ol>

如配置密码长度最小 8 位，至少包含大小写字母、数字、其它字符中的两类，并对 root 用户生效，配置如下：

<pre><code class="bash"># vi /etc/pam.d/system-auth
 password    requisite     pam_cracklib.so dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1 minclass=2 minlen=8  enforce_for_root
 password    sufficient    pam_unix.so md5 shadow nullok try_first_pass use_authtok
</code></pre>

> suse设备是/etc/pam.d/passwd文件 

### 密码不重复 5 次设置

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/pam.d/system-auth /etc/pam.d/system-auth_bak
</code></pre>

<ol start="2">
  <li>
    创建文件 /etc/security/opasswd，并设置权限：
  </li>
</ol>

<pre><code class="bash"># touch /etc/security/opasswd
# chown root:root /etc/security/opasswd
# chmod 600 /etc/security/opasswd
</code></pre>

<ol start="3">
  <li>
    修改策略设置，在 <code>password required pam_unix.so</code>所在行后面增加 <code>remember=5</code> ，保存退出；
  </li>
</ol>

<pre><code class="bash"># vi /etc/pam.d/system-auth
password required pam_unix.so remember=5 
</code></pre>

> 在相应行的后面添加 remember=5 ，而不是添加一行！ 

### 口令锁定策略

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/pam.d/system-auth /etc/pam.d/system-auth.bak
</code></pre>

<ol start="2">
  <li>
    修改策略设置：增加 <code>auth required pam_tally2.so even_deny_root deny=3 unlock_time=120</code> 到第二行，即 <code>auth required pam_env.so</code> 后面。
  </li>
</ol>

<pre><code class="bash"># vi /etc/pam.d/system-auth
 auth required pam_tally2.so even_deny_root deny=3 unlock_time=120 
</code></pre>

>   * even\_deny\_root：包含root用户
>   * deny：拒绝次数
>   * unlock_time：解锁时间

### 口令生存期

  1. 执行备份：

<pre><code class="bash"># cp -p /etc/login.defs /etc/login.defs.bak
</code></pre>

<ol start="2">
  <li>
    修改策略设置：修改 PASS_MIN_LEN 、 PASS_MAX_DAYS 、PASS_MIN_DAYS、PASS_WARN_AGE的值，保存退出。举例如下：
  </li>
</ol>

<pre><code class="bash"># vi /etc/login.defs
 PASS_MAX_DAYS   90           //口令最大有效天数
 PASS_MIN_DAYS   30          //口令最小有效天数
 PASS_MIN_LEN    8          //口令最少字符数
 PASS_WARN_AGE   7         //口令过期提前警告天
</code></pre>

### ftp 加固（若启用服务）

##### 更改 ftp 警告 Banner

  1. 修改vsftp回显信息

<pre><code class="bash"># vi /etc/vsftpd/vsftpd.conf
ftpd_banner=" Authorized users only. All activity may be monitored and reported."
</code></pre>

> 可根据实际需要修改告警文本内容。 

<ol start="2">
  <li>
    重启服务：
  </li>
</ol>

<pre><code class="bash"># service vsftpd restart
</code></pre>

##### ftp 用户对文件/目录的存取权限

> 前提：系统使用vsftp 

  1. 修改 /etc/vsftpd.conf 或 /etc/vsftpd/vsftpd.conf ，确保以下行未被注释掉，如果没有该行，请添加：

<pre><code class="bash"># vi /etc/vsftpd.conf
 write_enable=YES           //允许上传。如果不需要上传权限，此项可不进行更改。
 ls_recurse_enable=YES
 local_umask=022           //设置用户上传文件的属性为755
 anon_umask=022           //匿名用户上传文件(包括目录)的 umask
</code></pre>

<ol start="2">
  <li>
    重启 ftp 网络服务
  </li>
</ol>

<pre><code class="bash"># service vsftpd restart
</code></pre>

##### 限制FTP用户登录后能访问的目录

  1. 修改/etc/vsftpd.conf ，确保以下行未被注释掉，如果没有该行，请添加：

<pre><code class="bash"># vi /etc/vsftpd.conf
 chroot_local_user=YES
</code></pre>

<ol start="2">
  <li>
    重启网络服务
  </li>
</ol>

<pre><code class="bash"># service vsftpd restart
</code></pre>

##### 限制FTP用户登录

  1. 配置vsftpd.conf文件，设定只允许特定用户通过ftp登录，修改其中内容：

<pre><code class="bash"># vi /etc/vsftpd.conf
 userlist_enable=YES   // 此选项被激活后，VSFTPD 将读取 userlist_file 参数所指定的文件中的用户列表。
 userlist_deny=NO       // 决定禁止还是只允许由 userlist_file 指定文件中的用户登录FTP服务器。默认值 YES：禁止文件中的用户登录，同时也不向这些用户发出输入口令的提示；值为 NO：只允许在文件中的用户登录FTP服务器。
</code></pre>

> userlist\_file=/etc/vsftpd/user\_list 

<ol start="2">
  <li>
    判断条件
  </li>
</ol>

建议" root daemon bin sys adm lp uucp nuucp listen nobody noaccess nobody4 " 用户不能通过 FTP 登录。

### Telnet 告警信息设置(若启用服务)

  1. 修改 Telnet 回显信息，修改文件 /etc/issue 和 /etc/issue.net 中的内容：

<pre><code class="bash">#echo " Authorized users only. " &gt; /etc/issue
#echo " Authorized users only. " &gt; /etc/issue.net 
</code></pre>

> 可根据需要命令中的 “警告文本”。 

<ol start="2">
  <li>
    重启服务：
  </li>
</ol>

<pre><code class="bash"># service xinetd restart
</code></pre>

### 开启 iptables 防火墙

防火墙是网络安全的重中之重，有关 ipatables 的使用和帮助，将会另外写一篇详细描述。

<pre><code class="bash"># 开启 iptables 并设置开机自启动
# iptables service restart
# chkconfig iptables on
</code></pre>

下面是适合Web服务器的iptables规则示例和解释：

<pre><code class="bash"># IPT -P INPUT DROP #1
# IPT -P FORWARD DROP #1
# IPT -P OUTPUT DROP #1
# IPT -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT #2
# IPT -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT #3
# IPT -A INPUT -p tcp -m tcp --dport 22 -j ACCEPT #3
# IPT -A INPUT -i lo -j ACCEPT #4
# IPT -A OUTPUT -o lo -j ACCEPT #4
# IPT -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT #5
# IPT -A INPUT -p icmp -m icmp --icmp-type 11 -j ACCEPT #5
# IPT -A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT #6
</code></pre>

# 1、设置INPUT,FORWARD,OUTPUT链默认target为DROP，也就是外部与服务器不能通信。

# 2、设置当连接状态为RELATED和ESTABLISHED时，允许数据进入服务器。

# 3、设置外部客户端连接服务器端口80,22。

# 4、允许内部数据循回。

# 5、允许外部ping服务器。

# 6、设置状态为RELATED和ESTABLISHED的数据可以从服务器发送到外部。

### 禁止 ping 服务器

禁止 ping 服务器可以使用 iptables 进行设置，同时也可采用修改 `/etc/sysctl.conf` 禁止。

  * 编辑 sysctl.conf 文件，在文件中增加一行 `net.ipv4.icmp_echo_ignore_all=1` 
  * 如果已经有`net.ipv4.icmp_echo_ignore_all`这一行了，直接修改`=`号后面的值，0 表示允许，1 表示禁止
  * 修改完成后执行 `sysctl -p`使新配置生效。

### 检查是否记录安全事件日志

修改 /etc/syslog.conf 或 /etc/rsyslog.conf ，在文件中加入如下内容:

<pre><code class="bash"># vi  /etc/syslog.conf
 *.err;kern.debug;daemon.notice   /var/log/messages
# chmod 640 /var/log/messages
# service rsyslog restart 
</code></pre>

### 禁止普通用户合用 su 切换到 root 用户

1）把需要 su 到 root 的用户加入 wheel 组

<pre><code class="bash"># usermod -g  wheel test        // 例如把 test 用户加入 wheel 组
</code></pre>

2）修改配置文件/etc/pam.d/su ，去掉下面行的注释

<pre><code class="bash"># vi /etc/pam.d/su
 auth required pam_wheel.so use_uid       // 保证当前行不被注释
</code></pre>

3）配置生效后，仅 test 用户可使用 su 切换到 root 用户。

* * *

安全是一个永恒的话题，没有绝对的安全，只有不断加强的安全系统和体系。同时我们应该培养良好的安全意识和操作习惯。