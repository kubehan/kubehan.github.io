---
title: CentOS 7.5安装 GitLab
author: Kubehan
type: post
date: 2020-03-05T02:39:42+08:00
url: /509.html
views:
  - 951
  - 951
dwqr_like:
  - 2
  - 2
post_views_count:
  - 0
  - 0
zm_favorites:
  - 1
  - 1
categories:
  - Linux运维
tags:
  - Gitlab

---
\***\***\***\***\***\***\***\***\***\***\***\***\***\*****\*\\*\* CentOS 7.5安装 GitLab \*\*\***\***\***\***\***\***\***\***\***\***\***\***\***\***\***  
#!/bin/bash  
#相关包安装，  
yum -y install curl policycoreutils  
#这里可以不装，因为之前已经安装过  
#yum -y insyall openssh-server openssh-clients  
#systemctl enable sshd  
#systemctl start sshd  
#邮箱配置可以是别的，可选项  
yum install postfix  
systemctl enable postfix  
systemctl start postfix  
#防火墙配置  
#firewall-cmd --permanent --add-service=http  
#systemctl reload firewalld

touch /etc/yum.repos.d/gitlab_gitlab-ce.repo  
echo "  
[gitlab-ce]  
name=gitlab-ce  
baseurl=http://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7  
repo_gpgcheck=0  
gpgcheck=0  
enabled=1  
gpgkey=https://packages.gitlab.com/gpg.key  
" >> /etc/yum.repos.d/gitlab_gitlab-ce.repo

yum -y install gitlab-ce  
\# 添加访问的 host，修改/etc/gitlab/gitlab.rb的external_url  
\# external_url 修改为自己的地址  
sed -i 's/gitlab.example.com/47.94.204.35/g' /etc/gitlab/gitlab.rb  
#让配置生效  
gitlab-ctl reconfigure  
#重启gitlab  
gitlab-ctl restart  
##更换仓库存储路径  
#默认时GitLab的仓库存储位置在“/var/opt/gitlab/git-data/repositories”，在实际生产环境中显然我们不会存储在这个位置，一般都会划分一个独立的分区来存储仓库的数据，我这里规划把数据存放在“/data/git-data”目录下。  
mkdir -pv /data/git-data #看看自己的数据盘挂载哪里  
#启用git\_data\_dirs参数，并修改如下：  
\# git\_data\_dirs({  
\# "default" => {  
\# "path" => "/data/git-data"  
\# }  
\# })  
#  
echo "  
git\_data\_dirs({  
"default" => {  
"path" => "/data/gitlab"  
}  
})" >> /etc/gitlab/gitlab.rb

gitlab-ctl reconfigure #重新编译gitlab.rb文件，使用做的修改生效  
#重新编辑后，GitLab在仓库目录会自动创建一个repositories文件  
#重启gitlab  
gitlab-ctl restart

#配置邮件服务器  
echo "  
gitlab\_rails['smtp\_enable'] = true  
gitlab\_rails['smtp\_address'] = "smtp.163.com"  
gitlab\_rails['smtp\_port'] = 465  
gitlab\_rails['smtp\_user_name'] = "\***\***@163.com"  
gitlab\_rails['smtp\_password'] = "hanwei163com"  
gitlab\_rails['smtp\_domain'] = "192.168.1.98"  
gitlab\_rails['smtp\_authentication'] = "login"  
gitlab\_rails['smtp\_enable\_starttls\_auto'] = true  
gitlab\_rails['smtp\_tls'] = true  
gitlab\_rails['gitlab\_email_from'] = '\***\***95@163.com'  
user['git\_user\_email'] = "\***\***@163.com"  
">> /etc/gitlab/gitlab.rb  
#让配置生效  
gitlab-ctl reconfigure  
#重启gitlab  
gitlab-ctl restart  
#进控制台发送测试邮件  
gitlab-rails console  
irb(main):003:0> Notify.test\_email('hanw@oppo.com.cn', 'Message Subject', 'Message Body').deliver\_now

#成功后去web界面配置启用注册就行了