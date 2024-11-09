---
title: 如何把SVN迁移到Gitlab
author: Kubehan
type: post
date: 2020-03-04T10:25:25+08:00
url: /502.html
views:
  - 935
  - 935
wb_dl_type:
  - 0
  - 0
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
zm_like:
  - 1
  - 1
categories:
  - Linux运维
tags:
  - Gitlab

---
把SVN上的代码仓库迁移到Gitlab上，实际上就是把SVN仓库转变成Git仓库，并且希望能保留原SVN仓库的Commit等历史记录，这一点很重要。  
SVN迁移到Gitlab需要安装git-svn

<pre><code class="shell"># yum install -y git-svn
</code></pre>

保留原SVN仓库的Commit等历史记录，需要获取到SVN使用的作者名字列表，为了获得 SVN 使用的作者名字列表，可以在checkout到本地的仓库路径下运行这个：

<pre><code class="shell"># svn co --username tom --password 123456 http://my-project.googlecode.com/svn/  code
# cd code
# svn log --xml | grep author | sort -u | perl -pe 's/.*&gt;(.*?)&lt;.*/$1 = /' &gt; /root/users.txt
</code></pre>

这会将日志输出为 XML 格式，然后保留作者信息行、去除重复、去除 XML 标记。 然后，将输出重定向到你的 users.txt 文件中，这样就可以在每一个记录后面加入对应的 Git 用户数据，修改users.txt文件满足以下的格式：

<pre><code class="shell"># vim /root/users.txt
schacon = schacon &lt;schacon@geemail.com&gt;
selse =  selse &lt;selse@geemail.com&gt;
</code></pre>

然后开始把SVN仓库转变成Git仓库，执行以下命令：

<pre><code class="shell"># git svn clone http://my-project.googlecode.com/svn/   --authors-file=/root/users.txt  --no-metadata  my_project
</code></pre>

为了将标签变为合适的 Git 标签，运行

<pre><code class="shell"># cd  my_project
# cp -Rf .git/refs/remotes/origin/tags/* .git/refs/tags/
# rm -Rf .git/refs/remotes/origin/tags
</code></pre>

这会使原来在 remotes/origin/tags/ 里的远程分支引用变成真正的（轻量）标签。

接下来，将 refs/remotes 下剩余的引用移动为本地分支：

<pre><code class="shell"># cp -Rf .git/refs/remotes/* .git/refs/heads/
# rm -Rf .git/refs/remotes
</code></pre>

现在所有的旧分支都是真正的 Git 分支，并且所有的旧标签都是真正的 Git 标签。 最后一件要做的事情是，将你的新 Git 服务器添加为远程仓库并推送到上面。 下面是一个将你的服务器添加为远程仓库的例子：

<pre><code class="shell"># git remote add origin git@my-git-server:myrepository.git
</code></pre>

因为想要上传所有分支与标签，你现在可以运行：

<pre><code class="shell">$ git push origin --all
</code></pre>

通过以上漂亮、干净地导入操作，你的所有分支与标签都应该在新 Git 服务器上，你可以去gitlab上查看结果了。