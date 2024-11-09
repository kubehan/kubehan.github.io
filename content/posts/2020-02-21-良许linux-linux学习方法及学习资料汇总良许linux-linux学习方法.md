---
title: Linux学习方法及学习资料汇总
author: Kubehan
type: post
date: 2020-02-21T08:56:20+08:00
url: /260.html
wb_dl_type:
  - 1
  - 1
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
views:
  - 773
  - 773
post_views_count:
  - 0
  - 0
categories:
  - Linux运维

---
很多人想学习Linux，却不知道怎么着手，甚至不知道Linux有哪些方向，非常迷茫。基于此，我特地写了篇文章介绍Linux方向性问题，没想到一不小心成了爆款：

到什么程度才叫精通 Linux？​

<div class="image-package">
  <div class="image-container" style="max-width: 180px; max-height: 120px;">
    <div>
    </div>
    
    <div class="image-view" data-width="180" data-height="120">
    </div>
  </div>
  
  <div class="image-caption">
    图标
  </div>
</div>

看完这个回答，相信很多人至少知道了目前 Linux 从业者所从事的几个方向，对于方向选择有个大概的认知。

自我介绍一下。**我是良许，本科及硕士所学专业却是机械，毕业后从零开始自学转行 IT，1 年后被世界 500 强外企所录用，目前是 Linux 工程师**。

本文将根据我的从业经验及与同行大佬的交流，介绍一些Linux学习方法，**并且在文末赠送一些Linux书籍的电子版及及视频教程等资源**，希望对大家有帮助！

## 书籍篇

对于Linux书籍的推荐，我特地写了几个回答来介绍，这里就不重复贴回答了：

有没有学习Linux比较好的入门书籍？

求推荐学习linux命令的书籍?

有没有比《鸟哥的Linux私房菜》更好的书？

嵌入式Linux有哪些好书推荐？

## 资源篇

不管学习什么技术，资源都是必不可少的。想当年，我自学转行，靠的就是大量的优质资源。优质资源会助你一臂之力，让你快速入门。

当年我自学使用的优质资源，我也全部共享出来，在我的公众号「**良许Linux**」后台回复「**简书**」即可免费获取。

当然，我也整理了另外一些不错的资源，写在这个回答里了，大家可以看看：

有哪些好的Linux学习资源？​

<div class="image-package">
  <div class="image-container" style="max-width: 180px; max-height: 120px;">
    <div>
    </div>
    
    <div class="image-view" data-width="180" data-height="120">
    </div>
  </div>
  
  <div class="image-caption">
    图标
  </div>
</div>

## **如何入门并深入学习Linux**

**1. Linux学习路径**

Linux应用开发自学之路这篇文章介绍了我**从零开始自学转行Linux**的完整过程，被很多大号转载，并且影响了很多人，大家可以参考。

自学简单编程可行吗？这篇文章更详细介绍了我是如何自学转行的，包括心路历程，转行过程，转行中需要注意的地方，以及更高效转行成功的方法。

Linux 思维导图整理（建议收藏）这是一个技术大佬整理的Linux思维导图，包括：**Linux学习路径，Linux基础入门，Linux内核学习路线，Linux命令参考，Linux命令速查**等等。这份导图虽然不是100%全面，但如果能够将里面全部内容掌握下来，你也是个高手了。

<div class="image-package">
  <div class="image-container" style="max-width: 439px; max-height: 692px;">
    <div>
    </div>
    
    <div class="image-view" data-width="439" data-height="692">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

**2. Linux环境搭建**

**2.1 虚拟机安装与配置**

既然要学习Linux那肯定需要一个Linux环境。那么环境搭建有两个选择：**1. 安装虚拟机；2. 直接在实体机上安装**。对于这两个选择，我更倾向于第1个，因为前期学习一些命令及基础知识，直接在虚拟机上进行就可以了。

手把手教你安装Linux虚拟机

手把手教你配置Linux虚拟机

虚拟机常用的有两种：Vmware，VirtualBox。Vmware功能更强大，但是是收费的。而VirtualBox虽然功能不及Vmware，但对于新手完全够用了。这两篇文章所使用的是**Wmare**，手把手教你安装并配置虚拟机，图文并茂，一路跟下来就可以安装并配置好虚拟机，完成最基本的搭建。

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 529px;">
    <div>
    </div>
    
    <div class="image-view" data-width="720" data-height="529">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

**2.2 主机与虚拟机文件共享**

虚拟机安装好之后，还有一项很重要的工作要做，那就是实现虚拟机与主机的互通，也就是互相共享文件。实现文件共享有很多方式，一般而言有以下几种：

  * 使用 FTP 协议实现文件共享
  * 使用 samba 协议实现文件共享

特别地，对于Vmware有一套自己的专属文件共享方式，VirtualBox应该也有，但我没去研究过。对于Windows与Linux之间的文件共享，我们一般会用到一款很强大的共享工具——WinSCP，当然还有很多类似工具，比如**Xftp，FileZilla**。这些工具其实都是基于FTP协议，使用起来也大同小异，都非常方便。

<div class="image-package">
  <div class="image-container" style="max-width: 622px; max-height: 463px;">
    <div>
    </div>
    
    <div class="image-view" data-width="622" data-height="463">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

**2.3 终端工具**

作为一名Linux工程师，不管是运维还是开发，我们很多时间会是在命令行下工作。我一般是把虚拟机打开着，然后使用终端工具远程到虚拟机进行操作。这里推荐一款很强大的终端工具——MobaXterm，它的功能十分强大，界面也非常友好，我用上一次就爱不释手。

当然还有很多好用的终端工具，比如**XShell、secureCRT、Putty、telnet**等，选择一个自己最喜欢的工具即可。

**2.4 编程工具**

如果你是一名Linux开发人员，那你的工作肯定少不了编程。我一般的作法是，在Window上使用代码编辑工具编好代码，然后在Linux下编译。我经常使用两个工具：**Notepad++**和**Sourceinsight**。

使用notepad++远程编辑虚拟机文档

代码阅读神器——Sourceinsight

当然我们也可以直接在Linux下写代码，在Linux下编译。Linux下写代码也有很多软件，常用的比如最性感的编辑器——Sublime Text。

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 566px;">
    <div>
    </div>
    
    <div class="image-view" data-width="720" data-height="566">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

**3. Linux基础知识**

Linux环境搭建完毕之后，我们就可以正式进入到Linux的学习中来。

**3.1 Linux系统架构**

学习Linux，我们肯定要知道Linux的系统架构是怎样的。一般而言，Linux是由以下几部分构成：

  * 内核
  * bootloader
  * 文件系统
  * Shell
  * 应用程序

内核是Linux系统的核心，它往下直接与硬件打交道，向上连接应用程序。它是由Linux社区来共同维护，其中Linus是核心人物。内核主要是由**C语言及少量汇编语言**编写而成，是最著名的一个开源项目之一。内核的源码在这里，但对于初学者，就别指望能把它看懂。

初学者只要了解一些内核的基本架构即可，后期可以再进一步深入学习。网络上有一张非常经典的内核架构图，可以借助来理解内核。

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 706px;">
    <div>
    </div>
    
    <div class="image-view" data-width="720" data-height="726">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

Bootloader就是一个单片机程序，用来引导系统启动。这个程序比较简单，有些高手甚至自己手写了bootloader程序。

Linux文件系统有ext3、ext4等，而windows 有 fat32 、ntfs等。做底层开发的工程师需要深入了解，在此不赘述。

**3.2 Shell**

Shell是系统的用户界面，提供了用户与内核进行交互操作的一种接口（命令解释器）。它的基本作用如下图示：

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 347px;">
    <div>
    </div>
    
    <div class="image-view" data-width="720" data-height="347">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

对于Shell的使用，有非常多坑，初学者一定要规避。在此，我也找了两篇Shell面试题，以帮助大家学习进步：

必会的 24 道 Shell 脚本面试题

10 个实战与面试【常用 Shell 脚本】编写

**3.3 Vim**

Vim是Linux里非常重要的一个编辑器，但是，它比较难，对于初学者非常不友好，号称上古神器。Vim有很多命令，所以我们首先要学习Vim的基本命令。

如果你觉得Vim不好学，那么我介绍一款提高Vim水平的游戏。这款游戏灵感来自PacMan，让你使用Vim的命令去控制主角躲避怪物。把这款游戏玩熟练了，你的Vim水平也上了很大一个台阶。

当然如果是官方标配版的Vim，那其实还是非常不好用的。好在Vim社区有很多大神，他们开发了很多实用的插件，让Vim用起来不再那么难用，比如以下三款非常实用的插件：

Vim的三款实用插件

**3.4 其它**

除了以上3点，Linux系统还有很多基础知识，这些知识很多很细，没办法一篇文章讲完，需要在实践中慢慢学习。

比如Linux系统的目录结构，它是一个**树状结构**，跟Windows系统有本质的区别。

<div class="image-package">
  <div class="image-container" style="max-width: 675px; max-height: 392px;">
    <div>
    </div>
    
    <div class="image-view" data-width="675" data-height="392">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

还有，Linux有很多快捷键，掌握了这些快捷键会为我们提高不少效率。

虚拟终端快捷键

**4. Linux命令**

众所周知，Linux有非常多命令，但是，刚开始学Linux千万别贪多，别想着一口吃成大胖子。对于普通人而言，先学会一些最基本的命令，再去拓展其它更高阶的命令。

Linux命令基本格式及目录处理命令

超好用的Unix/Linux 命令技巧 大神为你详细解读

给Linux小白看的命令行极简教程

Linux的10个最危险的命令

**常用的命令可能就二三十个**，当你把这二三十个命令都用得非常熟之后，你才算刚入门。当然，你别小看这些基础命令，**很多基础命令有着自己的高级用法**，当你把高级用法都玩透了，你就开始慢慢脱离小白了。

5分钟 more 命令从入门到精通

Linux下 ls 命令的高级用法8例

Linux 下你所不知道的 7 个 SSH 命令用法

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 990px;">
    <div>
    </div>
    
    <div class="image-view" data-width="720" data-height="1018">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

**5. Linux神器**

在 Linux 下工作，有一些工具可能大大提高你的工作效率。有些工具大家可能耳熟能详，但还有更多神器可能连听都没听说过。

比如我们程序员经常需要绘制一些流程图，我们可以使用一些诸如EA之类的绘图工具，但这类工具很多都很庞大，而且比较难学。在Linux下其实我们可以使用**dot工具**简单高效绘图！

程序员轻松绘图神器

<div class="image-package">
  <div class="image-container" style="max-width: 299px; max-height: 577px;">
    <div>
    </div>
    
    <div class="image-view" data-width="299" data-height="577">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

再如，我们如果和同事协作开发，想要把自己的操作过程录制下来，发给同事然后同事继续你的工作。或者，别人使用你的电脑，你想把他的操作记录下来，以免后期争议。这时，Script命令就派上用场了。

Linux终端里的记录器

当然还有很多非常实用的神器，限于篇幅就不一一列举了。

如何高效回退到特定层级目录？

Linux任务的前后台管理

Linux下如何高效切换目录？

**6. Linux趣应用**

工作都是乏味的，我们要在工作中找到一些乐趣。作为一个免费的操作系统，大量的爱好者为 Linux 写了很多很有趣的应用，不仅可以帮助我们提高工作效率，而且还可以给我们枯燥的生活带来乐趣。

Linux 终端给人的感觉就是黑漆漆一片，里面只能显示一些字符，而从来没见过显示图片的，但是，实际上，Linux 终端除了显示字符外，当然也可以显示图片（然后就可以用来看女神照片）。那是怎么实现的呢？这篇文章有答案：

什么？Linux 终端也可以用来看女神照片？

<div class="image-package">
  <div class="image-container" style="max-width: 555px; max-height: 316px;">
    <div>
    </div>
    
    <div class="image-view" data-width="555" data-height="316">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

人这一辈子，真的是非常不容易：读书时，被老师、同学嘲笑，工作时，被老板、同事嘲笑，就连出去撸个串儿，还可能被朋友嘲笑……这些也就算了，毕竟大家还都是同类，都是活生生的人。但是，你如果被 Linux 终端给嘲笑了，你的内心会是什么感受？

说出来也许你不信，我被 Linux 终端嘲笑了…….

    [alvin@VM_0_16_centos ~]$ sldkf
    
      Why are you doing this to me?!
    
    -bash: sldkf: command not found
    [alvin@VM_0_16_centos ~]$ iehf
    
      You are not as bad as people say, you are much, much worse.
    
    -bash: iehf: command not found
    [alvin@VM_0_16_centos ~]$ sdfas
    
      How many times do I have to flush before you go away?
    
    -bash: sdfas: command not found
    

Git 是用来做啥的？想必码农朋友都知道，Git 是版本控制软件，是软件开发过程中团队协作不可或缺的软件。但是，作为版本控制软件的 Git ，能跟聊天工具扯上关系吗？这二者似乎毫无关系，但脑洞大开的外国朋友活生生将 Git 改造成了一个聊天工具！

Git 居然可以用来跟女神聊天？

<div class="image-package">
  <div class="image-container" style="max-width: 646px; max-height: 423px;">
    <div>
    </div>
    
    <div class="image-view" data-width="646" data-height="423">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

等等类似于此类的应用，这些应用虽然不是学习过程中的必需品，但却可以给我们的学习生活带来很多乐趣！

**7. Git**

作为程序员，肯定不是离开Git。Git是版本控制软件，是软件开发过程中团队协作不可或缺的软件。但可悲的是，在学校里很少会有Git相关课程，所以很多大学生都不知道有Git这个东西。

对于Git的入门，建议看 **Pro Git** 这本书，它是一本免费开源书，在它的官网上就可以直接在线阅读。

[[[[[[[[[[[[[[[<https://git-scm.com/book/zh/v2>][1]][1]][1]][1]][1]][1]][1]][1]][1]][1]][1]][1]][1]][1]][1]

<div class="image-package">
  <div class="image-container" style="max-width: 500px; max-height: 660px;">
    <div>
    </div>
    
    <div class="image-view" data-width="500" data-height="660">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

Git虽然命令也很多，但基本的常用的也没几个。在使用Git的过程中，我们也要注意一些 Git 提交规范。

如何高效的使用 Git

学会这两件事，让你成为 Git 老司机

你可能不太会用的 10 个 Git 命令

使用Git，就不得不提到**GitHub**。GitHub是一个面向开源及私有软件项目的托管平台，因为只支持git 作为唯一的版本库格式进行托管，故名GitHub。由于开发人员多为男性，故又名GayHub……

很多小伙伴知道使用Git，却不知道如何在GitHub上与其他小伙伴一起协作，为此我特地写了一篇文章来介绍**GitHub的协作方法**：

如何在GitHub上大显身手？

除此之外，还有你必须收藏的 GitHub 技巧

<div class="image-package">
  <div class="image-container" style="max-width: 427px; max-height: 369px;">
    <div>
    </div>
    
    <div class="image-view" data-width="427" data-height="369">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

## 后记

Linux博大精深，绝非一篇文章就能讲透的。本文根据自己的一些经验，介绍了一些方向性的东西。大家如果按着这些方向去学习，也一定会成为大神！

## 电子书+源码+精选Linux资料获取方法

在公众号「**良许Linux**」后台回复「**简书**」即可免费获取。

* * *

**❤️ 看完三件事：** 如果你觉得这篇内容对你挺有启发，我想邀请你帮我三个忙：

  1. **点赞，**让更多的人也能看到这篇内容（**收藏不点赞，都是耍流氓 -_-**）
  2. **关注我和专栏，**让我们成为长期关系
  3. **关注公众号「良许Linux」，**第一时间阅读最新的Linux文章，公众号后台回复 **1024** 送你 最新的编程技术资料。

<div class="image-package">
  <div class="image-container" style="max-width: 150px; max-height: 150px;">
    <div>
    </div>
    
    <div class="image-view" data-width="150" data-height="150">
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

 [1]: https://git-scm.com/book/zh/v2