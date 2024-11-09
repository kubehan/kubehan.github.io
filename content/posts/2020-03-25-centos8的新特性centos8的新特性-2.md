---
title: CentOS8的新特性
author: Kubehan
type: post
date: 2020-03-25T09:58:02+08:00
url: /1858.html
views:
  - 899
  - 899
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/frc-b531517104daa444e9e03109c86f61f3.jpg
  - https://www.kubehan.cn/wp-content/uploads/2020/03/frc-b531517104daa444e9e03109c86f61f3.jpg
cms_top:
  - 'true'
  - 'true'
cat_top:
  - 'true'
  - 'true'
categories:
  - Linux运维

---
## 一、CentOS8新特性**  
** 

* * *

    *   `CentOS8最终于2019年9月24日发布`。由于这是一个源自Red Hat Enterprise linux (RHEL)的Linux发行版，所以CentOS团队必须构建一个基础设施来支持新引入的RHEL 8。
    *   该版本还包含全新的`CentOS Streams`，Centos Stream是一个滚动发布的Linux发行版，它介于Fedora Linux的上游开发和RHEL的下游开发之间而存在。你可以把CentOS Streams当成是用来体验最新红帽系Linux特性的一个版本，而无需等太久。
    *   CentOS 8主要改动和 RedHat Enterprise Linux 8 是一致的，`基于Fedora 28和内核版本 4.18`,为用户提供一个稳定的、安全的、一致的基础，跨越混合云部署，支持传统和新兴的工作负载所需的工具
    *   CentOS系统在开发人员和系统管理员中广泛使用，因为它提供了对其高度可定制的开源软件包的完全控制。它是稳定的，背后有一个庞大而活跃的支持社区。由于其可靠性，它已经成为服务器操作系统的主流选择。
    

### 1. 两种模式：

    CentOS stream：滚动发布的 Linux 发行版，适用于需要频繁更新的开发者
    CentOS-1905：类似 RHEL 8 的稳定操作系统，系统管理员可以用其部署或配置服务和应用
    

### 2. 大致概述：

  * 扩展设备支持：

    GNOME现在集成了Thunderbolt 3连接支持。每当Thunderbolt 3建立连接并激活时，您将得到通知。该功能允许您密切监视所有连接，并检测任何安全漏洞或数据泄漏或盗窃企图。
    

  * 新的盒子特性：

    GNOME的应用程序中包含了一些用于管理远程和虚拟机的新特性。更新后的版本通过自动下载操作系统简化了创建虚拟环境的过程。此外，它的拖放功能可以让您轻松地在机器之间传输文件。
    

    *   新的屏幕键盘：
        GNOME团队重新编写了最新版本的屏幕键盘，试图解决紧迫的UI问题。现在，该功能支持多种布局以支持不同的地区、自动键盘激活和视图切换，因此用户在书写时可以清楚地看到文本。
    

  * 更新的UI界面：

        新的桌面环境还增加了几个额外的特性来改进UI和UX。这包括多显示器处理，直接窗口处理，改进的缩放，等等。
    

  * 网络功能，有两个主要的更新:

        CentOS现在提供了TCP网络堆栈版本4.16。
    

  * 使用的缺省包过滤框架是nftables。

        最重要的是，这些更改确保了更好的稳定性、可伸缩性和性能。
    

    nftables替代iptables、iptablesip6table、arptables和ebtables，作为IPv4和IPv6协议的单一框架。此外，firewalld deamon还将使用与默认后端相同的用于过滤网络事务的子系统。
    

### 3. Cockpit Web控制台

  * > 开放的基于web的控制台界面，Cockpit，现在作为新的CentOS发布的一部分。使用此平台可以通过web控制台界面轻松地管理服务器。通过web浏览器执行系统任务、创建和管理虚拟机、配置网络、启动容器和检查日志。

  * > Cockpit高度集成。它不仅有一个嵌入式终端，可以让你随时从终端切换到浏览器，而且还可以在移动设备上工作。

  * > 因此，当你安装CentOS 8时，它会自动设置Cockpit web控制台，并打开所需的防火墙端口。但是，不必担心它会加重系统的负担。该软件非常有效，因为它只在活动时使用内存和CPU。

### 4. 软件仓库更新

更加详细的官方文档

  * > 内容分布在两个主要的软件仓库:  
    > `BaseoS` repository  
    > `Appstream` Repository

  * > 虽然BaseOS包含所有底层OS包，但AppStream包含与应用程序相关的包、开发工具、数据库和其他包。

  * > BaseOS存储库拥有组成操作系统核心的传统RPM包。一旦更新了系统，它会自动下载并安装这些包的任何新版本。

  * > 有时可能不想批量升级软件，因为它可能会在你希望保持稳定的环境中导致兼容性问题(例如，在测试代码时)。这就是为什么新的CentOS 8附带了附加的AppStream存储库，提供了更多的特性、功能和定制。

  * > 这个软件仓库有一个不同的管理软件的方法，将它分为几个子类:  
    > Packages:作为常规包处理。  
    > Modules:相关或共享依赖项的包组。  
    > Module streams:软件模块的不同版本。  
    > Module profiles:构建模块的包的不同配置。

### 5. 软件管理更新

更新详细的官方文档

  * > CentOS 8附带`yum包管理器v4.0.4版本`，该版本`现在使用DNF (Dandified YUM)技术作为后端`。DNF是新一代的YUM，新的操作系统版本允许您同时使用这两种工具来管理包。

  * > 与DNF技术集成，最新版本有一个大大改进的软件管理系统。并`支持模块化内容`、`增强了性能`、并且`提供了设计良好的API`用于与其他工具集成。云应用程序流、容器工作负载和CI/CD。

### 6. 虚拟化更新

更详细的官方文档

  * > CentOS版本8带有KVM (qemu-kvm 2.12)，支持:

  * 5级分页功能，扩展了虚拟地址的大小，增加了可寻址的虚拟内存。
  * > 用户模式指令预防(UMIP)，一种将对用户空间应用程序的访问限制为系统级设置的安全特性。减少了特权升级攻击的潜在载体，从而使KVM虚拟机管理程序及其来宾计算机更加安全。

  * > `虚拟化支持Ceph存储`，在所有的RHEL CPU架构上提供块存储功能。

  * 所有虚拟机都预先设置的Q35机器类型(机器类型包括本机PCIe热插拔、IOMMU、安全启动和许多其他新集成的功能)。
  * > NVIDIA vGPU和VNC控制台之间的兼容性。

  * > QEMU仿真器引入的沙箱特性，以确保安全的代码测试。
    
    <div class="image-package">
      <div class="image-container" style="max-width: 700px; max-height: 420px;">
        <div>
        </div>
        
        <div class="image-view" data-width="1280" data-height="768">
          <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-8f5d635807a3d7bad82626b015c01ab3.jpg" alt="CentOS8的新特性CentOS8的新特性" />
        </div>
      </div>
      
      <div class="image-caption">
        image
      </div>
    </div>

### 7. 安全更新

  * > CentOS团队已经改进了安全特性，以确保数据保护和防止入侵。最新版本的OpenSSL 1.1.1默认包含TLS 1.3。这将确保您的所有数据以及客户端数据都受到密码保护。

  * > OS附带了一个全系统的加密策略，这意味着你不必修改各个应用程序的安全配置。

### 8. CentOS Stream

  * > CentOS Stream是一个结合了Fedora项目和RHEL的项目。CentOS团队创建了Stream版本作为滚动发行版，试图消除重大更新后的延迟和兼容性问题。

  * > 本项目的基本思路是:  
    > 通过一次更改几个包来获得开发更新Stream。  
    > 获取用户反馈并解决CentOS社区提出的任何问题。  
    > 允许在CentOS Stream上构建分层项目(如Ansible、oVirt和RDO)。

### 9. 命令行工具

更详细的官方文档

  * > `nobody用户更换nfsnobody`，这两个都已合并到nobody用户和组对中，使用65534 ID。新安装不再创建该nfsnobody对。此项更改减少了对nobodyNFS 拥有但与NFS无关的文件的困惑。

  * > 提供了版本控制系统  
    > `Git 2.18：`它是具有分散架构的分布式修订控制系统。
    
    > `Mercurial 4.8：`一种轻量级的分布式版本控制系统，旨在有效处理大型项目。
    
    > `Subversion 1.10：`一个集中的版本控制系统。

### 10. 编程语言 Web和数据库服务器

更详细的官方文档

  * **`Python 3.6：`默认情况下可能未安装该软件包。要安装Python 3.6，使用yum install python3命令**
  * **`Python 2.7：`继续支持，但是生命周期较短，其目的是促进使用者向Python3的平稳过渡**
  * **`Node.js` 是最新包含的，其他动态语言更新包括:** 
      * > `PHP 7.2：`  
        > 默认情况下使用FastCGI流程管理器,可以安全地与线程一起使用httpd  
        > 不再在使用httpd配置文件,应该在池配置中设置它：/etc/php-fpm.d/*.conf  
        > PHP脚本错误和警告记录到/var/log/php-fpm/www-error.log文件中，而不是/var/log/httpd/error.log  
        > 以下扩展已被删除： aspell ；mysql；memcache  
        > * Ruby 2.5；Perl 5.26；SWIG 3.0

  * > **提供`Apache HTTP Server 2.4`；** mod_http2提供了HTTP / 2支持。支持直接从PKCS#11模块中从硬件安全令牌加载TLS证书和私钥；以及首次引入的`nginx 1.14`

  * > **提供的数据库服务版本包括：**  
    > `MySQL 8.0`；  
    > 现在，它包含一个事务性数据字典，用于存储有关数据库对象的信息  
    > 默认字符集已从更改latin1为utf8mb4  
    > InnoDB现在支持带有锁定读取语句的NOWAIT和SKIP LOCKED选项  
    > `MariaDB 10.3`；  
    > 新增了这些功能常用表表达式；系统版本表；FOR 循环；隐形栏；顺序；即时ADD COLUMN的InnoDB；与存储引擎无关的列压缩；并行复制；多源复制。  
    > `PostgreSQL 9.6` & `PostgreSQL 10`；  
    > 顺序操作的并行执行：scan，join，和aggregate。  
    > 同步复制的增强。  
    > 改进的全文搜索，使用户可以搜索短语。  
    > 该postgres_fdw数据联合驱动程序现在支持远程join，sort，UPDATE，和DELETE操作。  
    > 大幅提升性能，尤其是在多CPU插槽服务器上的可伸缩性方面。  
    > `Redis 5`；

  * > `Squid 版本升级到4.4`，首次提供Varnish Cache 6.0  
    > Squid(用于Web客户端的高性能代理缓存服务器) ；varnish cache(一款高性能的开源HTTP加速器)

### 11. 桌面环境

更详细的官方文档

  * > GNOME Shell 升级到3.28.

  * > GNOME会话和显示管理使用 Wayland 作为默认的显示服务器，而RHEL 7默认的 X.Org server依然提供

### 12. 文件系统和存储

  * > `XFS`文件系统最大大小已从`500 TiB增加为1024 TiB`

  * > LUKS版本2（LUKS2）格式替代了旧版LUKS（LUKS1）格式；使用LUKS2作为加密卷的默认格式。LUKS2在部分元数据损坏事件的情况下为加密卷提供元数据冗余和自动恢复

### 13. 安全方面

更详细的官方文档

  * > 默认的系统级的 加密策略,用于配置核心加密子系统，覆盖TLS, IPsec, SSH, DNSSEC,和Kerberos协议。增加全新命令update-crypto-policies,管理员可以轻松切换不同模式：default, legacy, future,和fips.

  * > 支持智能卡和硬件安全模块 (HSM)的 PKCS #11

### 14. 网络方面

更详细的官方文档

  * > `nftables 框架替代 iptables` 作为默认的网络包过滤工具

  * > firewalld 守护进程使用 nftables 作为默认后端

  * > 支持 IPVLAN 虚拟网络驱动程序，可以为多个容器提供网络连接

### 15. 编译器和开发工具

更详细的官方文档

  * > `Gcc 编译器更新到8.2版本`，支持更多C++标准，更好的优化以及代码增强技术、提升警告和硬件特性支持

  * > 不同的代码生成、操作和调试工具现在可以处理 DWARF5 调试信息格式（体验阶段）

  * > 核心支持 eBPF 调试的工具包括BCC, PCP,和 SystemTap.

  * > \`glibc 库升级到2.28，支持 Unicode 11,更新的Linux系统调用，关键提升主要在DNS stub resolver、额外的安全加强和性能提升

  * > `提供OpenJDK 11, OpenJDK 8, IcedTea-Web,以及不同 Java 工具`，如 Ant, Maven,或 Scala.

## 安装CentOS8

### 1. 环境准备

#### **CentOS8-1905镜像**  
CentOS8-Stream的镜像

两个镜像的差异：

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 288px;">
    <div class="image-view" data-width="1137" data-height="288">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-e4ca9643c8b480edb5c724ead1182526.jpg" alt="CentOS8的新特性CentOS8的新特性" />
    </div>
  </div>
</div>

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 268px;">
    <div>
    </div>
    
    <div class="image-view" data-width="715" data-height="268">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-79c344e7416133b0adf1d4158eb52a02.jpg" alt="CentOS8的新特性CentOS8的新特性" />
    </div>
  </div>
</div>

<div class="image-package">
  <div class="image-container" style="max-width: 658px; max-height: 233px;">
    <div>
    </div>
    
    <div class="image-view" data-width="658" data-height="233">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-4a997f26d5516fd8bba4b8076dded19c.jpg" alt="CentOS8的新特性CentOS8的新特性" />
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

### 2. 安装步骤

VMware虚拟机：

> <div class="image-package">
>   <div class="image-container" style="max-width: 495px; max-height: 432px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="495" data-height="432">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-a1c88cf1d478d4671df2f33cf433b86f.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 499px; max-height: 433px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="499" data-height="433">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-f75e2108b685d1bac3827c787883901d.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 495px; max-height: 677px;">
>     <div class="image-view" data-width="495" data-height="677">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-8cfd168c79ac2e083696fef069c0a97e.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 503px; max-height: 435px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="503" data-height="435">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-309b2fb1e60a04a0a0d95862569ff647.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 502px; max-height: 434px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="502" data-height="434">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-2a61b0a732e4edac9ea7a0d34f892a8b.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> CentOS-8（1905）须要最少 2 GB 内存。官方推荐采用至少 4 GB 内存。
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 498px; max-height: 431px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="498" data-height="431">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-2d544fa367efc2d7fef4c56555066e5d.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 500px; max-height: 431px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="500" data-height="431">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-7d9c072938b349b37bbf43414e9c3c46.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 500px; max-height: 433px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="500" data-height="433">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-b3324e5d8fc24cb28181ad7c400ebe1a.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 499px; max-height: 435px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="499" data-height="435">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-3131015d2256ce99c6d8ccd1d1326a6d.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 500px; max-height: 432px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="500" data-height="432">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-bd7c16f612faf9a8324d7a0839046be9.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 319px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="816" data-height="319">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-3c6b48274346e290451fd99d9771fac1.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 选择NAT模式
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 431px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="726" data-height="431">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-eabf9854a192b9bd1093f36a0f9a0c1c.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 550px; max-height: 404px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="550" data-height="404">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-665dc779157de17478da3831d364eb04.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 进入虚拟机BIOS设置，将虚拟磁盘设置为第一启动项，F10保存
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 642px; max-height: 474px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="642" data-height="474">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-67bfd454882a9348adcbed427772b1f7.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 修改网卡名 tab键后输入 net.ifnames=0 biosdevname=0 然后回车
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 640px; max-height: 480px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="640" data-height="480">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-cb7e0380090c129a4dab7269f09617fd.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 建议选择英文，我这里选择的中文
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-1000be275aa256a7d5d4b4c1ceccd416.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-a201d235a65760b5153a55ab5f8107da.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>

<meta charset="utf-8">

> CentOS 8 分配了 40 GB 的硬盘空间。有两种分区方案可供选择：如果由安装向导进行自动分区，可以从“ 存储配置(Storage Configuration)”中选择“ 自动(Automatic)”选项；如果想要自己手动进行分区，可以选择“ 自定义(Custom)”选项。
> 
> 在这里我们选择“ 自定义(Custom)”选项，并按照以下的方式创建基于 LVM 的分区：
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-e516d20ebee2fcbb2ad4f02fc2e3da42.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-7116d57e56bafc2a4fb4abcf2cfee488.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-2a87b46fd990ddc557562d70e76f3191.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-07150f50b6dc02580d797a0ff08dfd20.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 可选择最小化或者带GUI的图形界面
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-4f557e8c6264ebc32be216b2c49bd459.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 最小化安装要选择的
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-31dc9a4f7b53aa406829dded46141d80.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 创建ROOT密码
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-879b6ca4ee5993fb58732843def0a0a1.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> 为虚拟机创建快照。
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-b531517104daa444e9e03109c86f61f3.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 600px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="800" data-height="600">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-f221ca3d1687a94bc4927d45acdbb891.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 420px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="1280" data-height="768">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-bb61cb2cc935b674f68b0a5568ba316f.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>

* * *

### 3. 可Cockpit Web控制台界面管理CentOS8

> CentOS8的一大改变就是通过Web直观的管理服务器的系统，这项新服务的名称为Cockpit,可以帮助不熟悉命令行的使用者管理服务器系统。不仅仅CenOS8 可以使用。ubuntu 和CentOS7 也是可以使用的。

  * 支持web终端，在web中关闭防火墙，selinux。
  * 支持虚拟机管理，需要安装cockpit-machines
  * 支持k8s dashboard管理，需要安装 cockpit-kubernetes
  * 支持web界面配置网卡

    systemctl start cockpit.socket
    systemctl enable cockpit.socket
    
    访问 http://IP:9090
    用户名密码和Linux的是一样的 打开Web界面后就可以实现对系统的监控和配置功能，Web界面的左侧便是其各种功能的菜单
    
    

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 393px;">
    <div>
    </div>
    
    <div class="image-view" data-width="1490" data-height="835">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-ea90246c12b1139acccb76b2d10256cb.jpg" alt="CentOS8的新特性CentOS8的新特性" />
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 370px;">
    <div>
    </div>
    
    <div class="image-view" data-width="1477" data-height="779">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-01a60662aee1da00d837570310996fcb.jpg" alt="CentOS8的新特性CentOS8的新特性" />
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

<div class="image-package">
  <div class="image-container" style="max-width: 700px; max-height: 386px;">
    <div>
    </div>
    
    <div class="image-view" data-width="1299" data-height="716">
      <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-9335bdf4884249dc1336f6ab505c8aef.jpg" alt="CentOS8的新特性CentOS8的新特性" />
    </div>
  </div>
  
  <div class="image-caption">
    image
  </div>
</div>

* * *

## 三、CentOS8的新操作

CentOS8系统优化快速上手

    [root@CentOS8 ~]# ssh -V
    OpenSSH_7.8p1, OpenSSL 1.1.1 FIPS  11 Sep 2018
    [root@CentOS8 ~]# cat /etc/redhat-release 
    CentOS Linux release 8.0.1905 (Core) 
    [root@CentOS8 ~]# uname -a
    Linux CentOS8 4.18.0-80.el8.x86_64 #1 SMP Tue Jun 4 09:19:46 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
    
    

### 3.1 网络配置—NetworkManager

> 在CentOS7上，同时支持network.service和NetworkManager.service。默认情况下，这2个服务都有开启，但许多人都会将NetworkManager禁用掉。

> 在CentOS8上，默认没有安装network.service，依然支持network。因此只能通过NetworkManager进行网络配置，包括动态ip和静态ip。换言之，在CentOS8上，必须开启NM，否则无法使用网络。

**NetworkManager介绍：**

  * > 能管理各种网络  
    > ▷ 有线网卡、无线网卡  
    > ▷ 动态ip、静态ip  
    > ▷ 以太网、非以太网  
    > ▷ 物理网卡、虚拟网卡

  * > 使用方法齐全  
    > ▷ nmcli：命令行。这是最常用的工具，本文将详细讲解该工具使用。  
    > ▷ nmtui：在shell终端开启文本图形界面。  
    > ▷ Freedesktop applet：如GNOME上自带的网络管理工具  
    > ▷ cockpit：redhat自带的基于web图形界面的"驾驶舱"工具，具有dashborad和基础管理功能。

  * > 为什么要用NetworkManager  
    > ▷ 工具齐全：命令行、文本界面、图形界面、web  
    > ▷ 广纳天地：纳管各种网络，有线、无线、物理、虚拟  
    > ▷ 参数丰富：多达200多项配置参数  
    > ▷ 一统江湖：RedHat、CentOS、Suse、Debian/Ubuntu，均支持  
    > ▷ 大势所趋：下一个大版本只能通过NM管理网络

* * *

### 3.2 设置网卡的新方法—nmcli

> 在NetworkManager里，有2个维度：连接（connection）和设备（device），这是多对一的关系。想给某个网卡配ip，首先NetworkManager要能纳管这个网卡。设备里存在的网卡（即nmcli d可以看到的），就是NM纳管的。接着，可以为一个设备配置多个连接（即nmcli c可以看到的），每个连接可以理解为一个ifcfg配置文件。同一时刻，一个设备只能有一个连接活跃。可以通过nmcli c up切换连接。

  * > `查看网卡连接信息`  
    > connection有2种状态：  
    > ▷ 活跃（带颜色字体）：表示当前该connection生效  
    > ▷ 非活跃（正常字体）：表示当前该connection不生效

    [root@CentOS8 ~]# nmcli connection
    NAME    UUID                                  TYPE      DEVICE 
    eth0    07b4217d-3fc4-48f7-ac70-1495cb856f98  ethernet  eth0   
    virbr0  4750c5db-fdd4-4423-812d-ab31120c88fd  bridge    virbr0 
    
    #查看connection列表和详细列表
    nmcli c show 
    nmcli c show eth0
    
    #语法：
    nmcli connection modify <interface_name> ipv4.address <ip/prefix>
    译作连接，可理解为配置文件，相当于ifcfg-eth0 可以简写为 nmcli c
    
    

  * > `获取网卡设备列表`  
    > device有4种常见状态：  
    > ▷ connected：已被NM纳管，并且当前有活跃的connection  
    > ▷ disconnected：已被NM纳管，但是当前没有活跃的connection  
    > ▷ unmanaged：未被NM纳管  
    > ▷ unavailable：不可用，NM无法纳管，通常出现于网卡link为down的时候

    [root@CentOS8 ~]# nmcli device 
    DEVICE  TYPE      STATE      CONNECTION 
    eth0    ethernet  connected  eth0       
    lo      loopback  unmanaged  -- 
    
    #查看所有device详细信息
    nmcli d show
    #查看指定device详细信息
    nmcli d show eth0
    
    译作设备，可理解为实际存在的网卡（包括物理网卡和虚拟网卡） 可以简写为 nmcli d
    
    

  * > `查看ip地址和NetworkManager状态`  
    > 类似于ifconfig、ip addr

    [root@CentOS8 ~]# nmcli 
    eth0: connected to eth0
            "Intel 82545EM"
            ethernet (e1000), 00:0C:29:B2:BE:2D, hw, mtu 1500
            ip4 default
            inet4 10.0.0.58/24
            route4 0.0.0.0/0
            route4 10.0.0.0/24
            inet6 fe80::b05:e02d:a476:ce8e/64
            route6 fe80::/64
            route6 ff00::/8
    
    lo: unmanaged
            "lo"
            loopback (unknown), 00:00:00:00:00:00, sw, mtu 65536
    
    DNS configuration:
            servers: 10.0.0.1
            interface: eth0
    
    Use "nmcli device show" to get complete information about known devices and
    "nmcli connection show" to get an overview on active connection profiles.
    
    Consult nmcli(1) and nmcli-examples(5) manual pages for complete usage details.
    
    

`启用和停止网卡连接`

    #相当于ifup ifdown
    启用  nmcli c up eth0
    停止  nmcli c down eth0
    
    

`立即生效connection，有3种方法`

    nmcli c up eth0
    nmcli d reapply eth0
    nmcli d connect eth0
    
    

`删除网卡连接`

    nmcli c delete eth0
    
    

`重载所有、指定 ifcfg或route到connection`（不会立即生效）

    nmcli c reload
    
    nmcli c load /etc/sysconfig/network-scripts/ifcfg-eth0
    
    

`创建网络连接并配置静态IP地址`（等同于配置ifcfg，其中BOOTPROTO=none，并ifup启动）

    nmcli c add type ethernet con-name eth0 ifname eth0 ipv4.addr 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.method manual
    
    

`创建网络连接并配置动态IP地址`（等同于配置ifcfg，其中BOOTPROTO=dhcp，并ifup启动）

    nmcli c add type ethernet con-name eth0 ifname eth0 ipv4.method auto
    
    

`修改IP地址`（非交互式）

    nmcli c modify eth0 ipv4.addr '192.168.0.200/24'
    nmcli c up eth0
    
    

`修改IP地址`（交互式）

    nmcli c edit eth0
    nmcli> goto ipv4.addresses
    nmcli ipv4.addresses> change
    Edit 'addresses' value: 192.168.1.200/24
    Do you also want to set 'ipv4.method' to 'manual'? [yes]: yes
    nmcli ipv4> save
    nmcli ipv4> activate
    nmcli ipv4> quit
    
    

`关闭无线网络`

    nmcli r all off
    
    

`查看NetworkManager状态`

    nmcli n 
    
    #开启NetworkManager
    nmcli n on
    #关闭NetworkManager
    nmcli n off
    
    

`监听事件`

    nmcli m 
    
    

`检测NetworkMarger是否在线可用`

    nm-online 
    
    

`例子：创建一个连接（connection）`

    nmcli c add type ethernet con-name eth0-test ifname eth0-test ipv4.addresses '192.168.1.100/24,192.168.1.101/32' ipv4.routes '10.0.0.0/8 192.168.1.10,192.168.0.0/16 192.168.1.11' ipv4.gateway 192.168.1.254 ipv4.dns '8.8.8.8,4.4.4.4' ipv4.method manual
    
    ▪ type ethernet：创建连接时候必须指定类型，类型有很多，可以通过nmcli c add type -h看到，这里指定为ethernet。
    ▪ con-name ethX ifname ethX：第一个ethX表示连接（connection）的名字，这个名字可以任意定义，无需和网卡名相同；第二个ethX表示网卡名，这个ethX必须是在nmcli d里能看到的。
    ▪ ipv4.addresses '192.168.1.100/24,192.168.1.101/32'：配置2个ip地址，分别为192.168.1.100/24和192.168.1.101/32
    ▪ ipv4.gateway 192.168.1.254：网关为192.168.1.254
    ▪ ipv4.dns '8.8.8.8,4.4.4.4'：dns为8.8.8.8和4.4.4.4
    ▪ ipv4.method manual：配置静态IP
    
    对应生成的网卡配置文件就是
    [root@CentOS8 ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0-test 
    TYPE=Ethernet
    PROXY_METHOD=none
    BROWSER_ONLY=no
    BOOTPROTO=none
    IPADDR=192.168.1.100
    PREFIX=24
    IPADDR1=192.168.1.101
    PREFIX1=32
    GATEWAY=192.168.1.254
    DNS1=8.8.8.8
    DNS2=4.4.4.4
    DEFROUTE=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_FAILURE_FATAL=no
    IPV6_ADDR_GEN_MODE=stable-privacy
    NAME=eth0-test
    UUID=ced95bb1-d856-4ace-8f2e-79a3f2d572c6
    DEVICE=eth0-test
    ONBOOT=yes
    
    #通过该nmcli c 可以看到增加了一条连接
    [root@CentOS8 ~]# nmcli c
    NAME       UUID                                  TYPE      DEVICE 
    eth0       6eebabfa-8f58-40af-b298-a385094b2f04  ethernet  eth0   
    eth0-test  ced95bb1-d856-4ace-8f2e-79a3f2d572c6  ethernet  --     
    
    

* * *

### 3.3 使用nmtui命令管理网卡

    nmtui
    nmtui edit eth0
    
    

> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 572px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="1128" data-height="572">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-b8135593697761c675e44dbaca531a35.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>
> 
> <div class="image-package">
>   <div class="image-container" style="max-width: 700px; max-height: 572px;">
>     <div>
>
>     </div>
>     
>     <div class="image-view" data-width="1128" data-height="572">
>       <img decoding="async" class="aligncenter" src="https://www.kubehan.cn/wp-content/uploads/2020/03/frc-f1b12ebd6b8c89d087e87e1c327321da.jpg" alt="CentOS8的新特性CentOS8的新特性" />
>     </div>
>   </div>
>   
>   <div class="image-caption">
>     image
>   </div>
> </div>

### 3.4 配置yum/dnf 源

CentOS 8 默认是会读取centos.org的mirrorlist的，所以一般来说是不需要配置镜像的。

    cd /etc/yum.repos.d
    #备份
    cp CentOS-Base.repo CentOS-Base.repo.bak
    cp CentOS-AppStream.repo CentOS-AppStream.repo.bak
    cp CentOS-Extras.repo CentOS-Extras.repo.bak
    
    ---
    vim /etc/yum.repos.d/CentOS-Base.repo
    [BaseOS]
    name=CentOS-$releasever - Base
    baseurl=https://mirrors.aliyun.com/centos/$releasever/BaseOS/$basearch/os/
    gpgcheck=1
    enabled=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
    
    ---
    vim /etc/yum.repos.d/CentOS-APPStream.repo
    [AppStream]
    name=CentOS-$releasever - AppStream
    baseurl=https://mirrors.aliyun.com/centos/$releasever/AppStream/$basearch/os/
    gpgcheck=1
    enabled=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficia
    
    ---
    vim /etc/yum.repos.d/CentOS-Extras.repo
    [extras]
    name=CentOS-$releasever - Extras
    baseurl=https://mirrors.aliyun.com/centos/$releasever/extras/$basearch/os/
    gpgcheck=1
    enabled=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
    
    

### 3.5 关闭selinux firewalld

    setenforce 0
    systemctl stop firewalld
    systemctl disable firewalld
    
    

### 3.6 安装tab补全插件

    yum install bash-completion
    source /etc/profile
    
    

### 3.7 最小化系统安装Cockpit Web控制台

    dnf install cockpit -y
    systemctl start cockpit
    netstat -lntup|grep 9090
    
    浏览器访问 https://ip:9090
    
    

### 3.8 常用软件包安装

> 对开发者的工具支持，可以用原生yum源安装了，不再需要通过第三方yum源。  
> @注意：CentOS8 严格区分pip2 和pip3，python2-pip和python3-pip

`dnf install python3 php perl java-1.8.0 java-11 maven docker mysql-server mlocate lrzsz tree vim nc nmap wget bash-completion htop iotop iftop lsof net-tools sysstat unzip bc psmisc telnet-server -y`

### 3.9 nftables防火墙规则

更详细的官方文档

>   * nftables 框架替换了 iptables 默认网络数据包过滤工具，可以通过`nft`命令可编程式的配置防火墙。
>   * firewalld也是Linux的防火墙，同时支持iptables和nftables，最新版本默认使用nftables。
>   * 简单的说firewalld是基于nftfilter防火墙的用户界面工具。而iptables和nftables是命令行工具。
>   * firewalld引入区域的概念，可以动态配置，让防火墙配置及使用变得简便。
>   * 从用户的角度来看，nftables添加了一个名为nft的新工具，它取代了iptables，arptables和ebtables中的所有其他工具。
>   * 从架构的角度来看，它还取代了处理包过滤规则集的运行时评估的内核部分。

`查询所有表名`

    [root@CentOS8 ~]# nft list tables
    table ip filter
    table ip6 filter
    table bridge filter
    table ip security
    table ip raw
    table ip mangle
    table ip nat
    table ip6 security
    table ip6 raw
    table ip6 mangle
    table ip6 nat
    table bridge nat
    
    

`查看某个表的规则`

    [root@CentOS8 ~]# nft list table filter
    table ip filter {
        chain INPUT {
            type filter hook input priority 0; policy accept;
        }
    
        chain FORWARD {
            type filter hook forward priority 0; policy accept;
        }
    
        chain OUTPUT {
            type filter hook output priority 0; policy accept;
        }
    }
    
    

`打开交互配置模式`

    nft -i
    
    

### 3.10 nft基础操作

云栖社区-linux nftables简介和基础操作

1、增

    增加表：nft add table fillter
    增加链：nft add chain filter input { type filter hook input priority 0 ; } # 要和hook（钩子）相关连
    增加规则：nft add rule filter input tcp dport 22 accept
    
    

2、删

    只需要把上面的 add 改为 delete 即可
    
    

3、改

    更改链名用rename
    更改规则用replace
    
    

4、查

    nft list ruleset # 列出所有规则
    nft list tables # 列出所有表
    nft list table filter # 列出filter表
    nft chain filter input # 列出filter表input链
    以上命令后面也可以加 -nn  用于不解析ip地址和端口
    加 -a 用于显示 handles