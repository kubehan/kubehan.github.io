---
title: Confluence简单使用教程
author: Kubehan
type: post
date: 2020-03-24T04:03:01+08:00
url: /1511.html
Baidusubmit:
  - 1
  - 1
views:
  - 2010
  - 2010
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence3.png
  - https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence3.png
categories:
  - Linux运维

---
<div style="text-align: justify;">
  <h2>
    1.Confluence简介
  </h2>
</div>

Confluence地址：<http://192.168.1.214:8090/>

<span style="background-color: white;">Confluence是一个企业级的Wiki软件，可用于在企业、部门、团队内部进行信息共享和协同编辑。</span>

### 1.1 能做什么？

团队将日常的工作任务进度管理、技术知识分享、开发文档、操作手册、文档附件都放置入Confluence中进行统一管理，富文本编辑器使文档内容支持更加丰富。

### 1.2 基础概念

Confluence的使用并不复杂，只需掌握如下几个基础概念。空间（Space）空间是Confluence系统中的一个区域，用于存储wiki页面，并可实现对空间中的所有文档进行统一的权限管理。

通常，我们可以针对每个项目单独创建一个空间，然后将与该项目相关的文档信息放置到该空间中，并只对项目成员开设访问/编辑权限。

除了项目空间，每个成员都有一个个人空间。平时成员可以将工作总结或笔记等文档放置到自己的空间中；对于对团队有帮助的文档，就可以将文档移动至团队项目空间中

#### 1.2.1 Dashboard

Dashboard是Confluence系统的主页，在Dashboard界面中包含了Confluence站点中的所有空间列表，以及最近更新内容的列表。

#### 1.2.2 页面（Page）

在Confluence系统中，页面是存储和共享信息的主要方式。

页面可以互相链接、连接、组织和访问，并以树状结构进行组织，放置于空间之中。页面遵循所见即所得的编辑方式，操作上简单易用。更强大的地方在于，页面支持大量的内容展现形式，除了富文本文档外，还包括图表、视频、附件（可预览）、流程图、公式等等；如果还不够，还可以通过海量的第三方插件进行扩展。

在页面中可以通过@其它成员，通知相关成员查看文档。文档保存成功后，被@的成员就会收到邮件，并可根据邮件中的链接访问到该文档，然后进行评论或者协同编辑

#### 1.2.3 模板（template）

创建页面时除了采用空白文档，也可以选择模板。模板是在空白文档的基础上，根据特定需求添加了一些文档要素，可辅助用户更好更快地创建文档。

Confluence内置了大量的模板，可辅助用于项目工作的各个环节，包括产品需求、会议记录、决策记录、指导手册（How-to）、回顾记录、工作计划、任务报告等等。并且由于Confluence和JIRA是同一家公司的产品，在Confluence中可以和JIRA进行无缝衔接，实现对产品质量实现更好的展现。

如果对Confluence自带的模板不满意，还可以对模板进行调整，或者根据自己的需求创建其它类型的模板。

#### 1.2.4 权限（Permission）

在安全性方面，Confluence具有完善和精细的权限控制，可以很好地控制用户在Wiki中创建、编辑内容和添加注释。

权限控制分3个维度，分别是团队（Group），个人（Individual Users），匿名用户（Anonymous）。

使用团队级的权限控制时，需要在Confluence服务器中对公司员工进行分组，好处在于配置比较方便，只需要对整个团队进行统一的权限配置。

但在实际项目中，经常会存在同一个项目包含多个跨团队成员的情况，这个时候就不适合采用团队权限配置方式，只能采用逐个添加成员的方式，并对各个成员分别配置权限。

另外一种情况，就是对于未登录的用户，以及项目成员以外的用户，可以开设部分权限，例如只读（View）。

<div style="text-align: justify;">
  <h2>
    注册用户
  </h2>
</div>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence1.png" alt="" />  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence2.png" alt="" /> 

<div style="text-align: justify;">
  <h2>
    创建空间
  </h2>
</div>

新建一个项目时，首先要做的就是创建一个空间，并进行初始化配置。

创建空间的方式很简单，可以从顶部菜单进行创建：【空间】->【创建】；也可以从Dashboard页面的Spaces页面中进行创建。

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence3.png" alt="" /> 

  1. <div style="text-align: justify;">
      <span style="font-size: 14pt;">进入创建空间页面后，需要选择空间类型。这个需要根据空间的用途进行选择，对于团队协作的空间，推荐选择"Team Space"，如果实在不知道选择什么类型，选择"Blank Space"也是可以的。<br /> </span>
    </div>

  2. <div style="text-align: justify;">
      <span style="font-size: 14pt;">然后是填写空间的基本信息。所有类型的空间都有两个必填字段，空间名和空间标识符。空间标识符可以理解为空间的ID，不同空间的空间标识符不能重复，但空间名是可以重复的。<br /> </span>
    </div>

  3. <div style="text-align: justify;">
      <span style="font-size: 14pt;">另外，对于"Team Space"类型的空间，多了一个"Team members"字段，用于添加空间的成员。成员的名称是其公司邮箱的前缀。<br /> </span>
    </div>

需要说明的是，空间创建完成后，Space key字段是不能修改的，其它字段以及团队成员都可以进行修改。

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence4.png" alt="" />  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence5.png" alt="" /> 

<div style="text-align: justify;">
  <h2>
    配置空间权限
  </h2>
</div>

创建空间后，根据项目需要，可以给空间设置权限。只有空间的管理员才能对空间权限进行配置。

操作方式如下：首先进入空间的页面，在空间左下角中，【空间管理】->【权限】，进入权限管理页面。

Confluence的权限控制比较完善，可以根据团队规范进行较为精细粒度的设置。

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence6.png" alt="" />  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence7.png" alt="" /> 

<div style="text-align: justify;">
  <h2>
    常见操作
  </h2>
</div>

熟悉了Confluence的基础概念，基本上就可以摸索着对Confluence进行上手了。不过，为了减少摸索时间，在这里我再将Confluence中的常用操作进行说明。

### 4.1文档查看与操作

用户账户登录首页->选择要查看的【More】->【空间】-->选择对应的项目→查看您想看的文档

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence8.png" alt="" /> 

也可以是这样的页面

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence9.png" alt="" /> 

### 4.2 添加文档

在Confluence中文件以树状结构进行组织。

推荐的创建方式是，先进入父目录的页面，然后再点击【Create】进行创建。在创建文档页面中，可以看到新建文档的"父级"，表示新文档创建后将位于"父级"文件的下一个层级中。

在新建文档时，需要选择文档模板。这个就根据文档的实际类型或用途进行选择即可，如果觉得都不合适，就选择"Blank page"。

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence10.png" alt="" /> 

### 4.3 编写文档

在编写文档时，页面遵循所见即所得的编辑方式，基本上跟在MS Word中的操作类似。  
Confluence也集成了许多编辑工具，可以很方便地插入图表、链接、附件、代办列表等等。如果还不满足需求，可以点击【插入】->【其他宏】，查找更多的扩展插件。

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence11.png" alt="" /> 

#### <span style="background-color: white;">4.3.1 插入sql<br /> </span>

<span style="font-family: 微软雅黑; background-color: white;">选择【格式化】->【代码块】->【语法高亮】->自由选择风格【选择sql】->自由选择主题【选择】->【emacs】,接着确认，即可插入sql<br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence12.png" alt="" /> <span style="font-family: 微软雅黑; background-color: white;"><br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence13.png" alt="" /> <span style="font-family: 微软雅黑; background-color: white;"><br /> </span>

#### <span style="background-color: white;">4.3.2 插入表格，<br /> </span>

<span style="font-family: 微软雅黑; background-color: white;">插入开发状态颜色，点击【+】->选择【状态】<br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence14.png" alt="" /> 

#### 4.3.3 插入图片和附件

选择【插入文件和图片】

例如，Confluence默认是不支持Markdown编辑模式的，如果想采用Markdown来编写文档，就可以通过上述方式到插件市场寻找Markdown的插件。

不过根据实践发现，当前Confluence的Markdown插件支持的还不够好，使用体验上不尽如人意。比较推荐的做法，还是在单独的Markdown编辑器上采用markdown语法进行编辑，编辑完成后进行预览，然后将渲染后的文档内容复制粘贴到Confluence中。

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence15.png" alt="" /> 

#### <span style="background-color: white;">4.3.4 移动文档<br /> </span>

很多时候我们需要调整目录结构，这就涉及到需要将文档移动到别的目录层级下。  
操作方式如下：先进入到待移动的文档页面中，点击页面右上角的【…】->【移动】；  
然后选择移动到新的目录即可

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence16.png" alt="" /> 

### 4.4 创建目录

<span style="color: #4d4d4d; font-family: 微软雅黑; background-color: white;">与创建文档方式一致，直接创建标题保存即可</span>

### 4.5 模板方式创建文档

<span style="color: #4d4d4d; font-family: 微软雅黑; background-color: white;">选择【以层级方式查看】->[模板]->【编辑】<br /> </span>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence17.png" alt="" /><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence18.png" alt="" /> 

<div style="text-align: justify;">
  <h2>
    发表评论
  </h2>
</div>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence19.png" alt="" />  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence20.png" alt="" /> 

<div style="text-align: justify;">
  <h2>
    上传文档附件
  </h2>
</div>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence21.png" alt="" />  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence22.png" alt="" /> 

<div style="text-align: justify;">
  <h2>
    导出
  </h2>
</div>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence23.png" alt="" /> 

<div style="text-align: justify;">
  <h2>
    文档权限管理
  </h2>
</div>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence24.png" alt="" />  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/032420_0402_Confluence25.png" alt="" />