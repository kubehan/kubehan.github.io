---
title: Linux特殊权限rws、rwS和rwt、rwT
author: Kubehan
type: post
date: 2020-05-13T03:32:31+08:00
excerpt: |
  日常生活中我见得最多的基本上就是rwx权限了，但是还有一部分特殊权限们这里记录一下
  
  1.S权限（SETUID）
  何为s权限？set uid 设置uid，也就是 让普通用户可以以root用户的角色运行
url: /2567.html
Baidusubmit:
  - 1
views:
  - 2496
cms_top:
  - 'true'
cat_top:
  - 'true'
post_img:
  - 'true'
hot:
  - 'true'
zm_like:
  - 2
categories:
  - Linux运维

---
日常生活中我见得最多的基本上就是rwx权限了，但是还有一部分特殊权限们这里记录一下

* * *

#### 1.s权限（setuid）

何为s权限？set uid 设置uid，也就是 让普通用户可以以root用户的角色运行只有root帐号才能运行的程序或命令

值得我们注意的是： 文件属主和组设置SUID和GUID，文件在被设置了s权限后将以root身份执行。在设置s权限时文件属主、属组必须先设置相应的x权限，否则s权限并不能正真生效。

## 实操演示：

<pre><code class="language-bash">[root@pythondesign ~]# ls -ld install.sh 
-rw-r--r-- 1 root root 20315 Apr  1 15:14 install.sh
[root@pythondesign ~]# chmod u+s install.sh 
[root@pythondesign ~]# ls -ld install.sh 
-rwSr--r-- 1 root root 20315 Apr  1 15:14 install.sh
[root@pythondesign ~]# chmod 755 install.sh 
[root@pythondesign ~]# ls -ld install.sh 
-rwxr-xr-x 1 root root 20315 Apr  1 15:14 install.sh
[root@pythondesign ~]# chmod u+s install.sh 
[root@pythondesign ~]# ls -ld install.sh 
-rwsr-xr-x 1 root root 20315 Apr  1 15:14 install.sh
[root@pythondesign ~]# </code></pre>

* * *

通过上面几个演示可以看出，在我们没有对文件添加x权限时候s权限是大写的S，添加x权限也就是添加755权限之后就变成了小写的s，只有变成小写的s，权限才能生效，Linux中passwd修改密码这个命令就是s权限的最好体现， Linux修改密码的passwd便是个设置了SUID的程序，普通用户无读写/etc/shadow文件的权限确可以修改自己的密码。

<pre><code class="language-shell">[root@pythondesign ~]# ls -l which passwd
-rwsr-xr-x. 1 root root 34928 May 11  2019 /usr/bin/passwd
[root@pythondesign ~]# </code></pre>

这里如果没有了s权限，那么普通用户只有通过root来修改密码，这样root就知道了我普通用户的密码，这样是不行的。如果给用户设置成rw权限，那么用户就可以通过删除/etc/shadow里的root的密码，来让root无密码，可直接登陆。这样就会造成极大的风险 

## `设置方法：chmod u+s filename or command`

#### 2.粘滞位（t）

何为t权限？ 只能由属主和root来执行删除文件的权限 ，和上面一样也需要先有x权限，当没有x权限的时候，为大写T ，设置粘滞位的文件，只能由以下账户删除

  1. 超级管理员
  2. 该目录的所有者
  3. 该文件的所有者

## `权限设置方法：chmod u+t  filename or command`

#### 3.其他特殊权限

i：不可修改权限  
例：chattr u+i filename 则filename文件就不可修改，无论任何人，如果需要修改需要先删除i权限，用chattr -i filename就可以了。查看文件是否设置了i权限用lsattr filename。

a：只追加权限， 对于日志系统很好用，这个权限让目标文件只能追加，不能删除，而且不能通过编辑器追加。可以使用chattr +a设置追加权限