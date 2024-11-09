---
title: 使用 GitLab 实现 CI/CD
author: Kubehan
type: post
date: 2020-12-20T08:25:01+08:00
url: /3120.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 789
categories:
  - Linux运维

---
# 使用 GitLab 实现 CI/CD

GitLab CI/CD 是一个内置在GitLab中的工具，用于通过持续方法进行软件开发：  
Continuous Integration (CI) 持续集成  
Continuous Delivery (CD) 持续交付  
Continuous Deployment (CD) 持续部署  
持续集成的工作原理是将小的代码块推送到Git仓库中托管的应用程序代码库中，并且每次推送时，都要运行一系列脚本来构建、测试和验证代码更改，然后再将其合并到主分支中。

持续交付和部署相当于更进一步的CI，可以在每次推送到仓库默认分支的同时将应用程序部署到生产环境。

这些方法使得可以在开发周期的早期发现bugs和errors，从而确保部署到生产环境的所有代码都符合为应用程序建立的代码标准。

GitLab CI/CD 由一个名为 .gitlab-ci.yml 的文件进行配置，改文件位于仓库的根目录下。文件中指定的脚本由GitLab Runner执行。  
GitLab CI/CD 介绍

软件开发的持续方法基于自动执行脚本，以最大程度地减少在开发应用程序时引入错误的机会。从开发新代码到部署新代码，他们几乎不需要人工干预，甚至根本不需要干预。 

它涉及到在每次小的迭代中就不断地构建、测试和部署代码更改，从而减少了基于已经存在bug或失败的先前版本开发新代码的机会。

Continuous Integration（持续集成）  
假设一个应用程序，其代码存储在GitLab的Git仓库中。开发人员每天都要多次推送代码更改。对于每次向仓库的推送，你都可以创建一组脚本来自动构建和测试你的应用程序，从而减少了向应用程序引入错误的机会。这种做法称为持续集成，对于提交给应用程序（甚至是开发分支）的每项更改，它都会自动连续进行构建和测试，以确保所引入的更改通过你为应用程序建立的所有测试，准则和代码合规性标准。 

Continuous Delivery（持续交付）  
持续交付是超越持续集成的更进一步的操作。应用程序不仅会在推送到代码库的每次代码更改时进行构建和测试，而且，尽管部署是手动触发的，但作为一个附加步骤，它也可以连续部署。此方法可确保自动检查代码，但需要人工干预才能从策略上手动触发以必输此次变更。

Continuous Deployment（持续部署）  
与持续交付类似，但不同之处在于，你无需将其手动部署，而是将其设置为自动部署。完全不需要人工干预即可部署你的应用程序。 

1.1. GitLab CI/CD 是如何工作的  
为了使用GitLab CI/CD，你需要一个托管在GitLab上的应用程序代码库，并且在根目录中的.gitlab-ci.yml文件中指定构建、测试和部署的脚本。

在这个文件中，你可以定义要运行的脚本，定义包含的依赖项，选择要按顺序运行的命令和要并行运行的命令，定义要在何处部署应用程序，以及指定是否 要自动运行脚本或手动触发脚本。 

为了可视化处理过程，假设添加到配置文件中的所有脚本与在计算机的终端上运行的命令相同。

一旦你已经添加了.gitlab-ci.yml到仓库中，GitLab将检测到该文件，并使用名为GitLab Runner的工具运行你的脚本。该工具的操作与终端类似。

这些脚本被分组到jobs，它们共同组成一个pipeline。一个最简单的.gitlab-ci.yml文件可能是这样的：  
before_script:

  * apt-get install rubygems ruby-dev -y

run-test:  
script:

  * ruby --version 6  
    before_script属性将在运行任何内容之前为你的应用安装依赖，一个名为run-test的job（作业）将打印当前系统的Ruby版本。二者共同构成了在每次推送到仓库的任何分支时都会被触发的pipeline（管道）。

GitLab CI/CD不仅可以执行你设置的job，还可以显示执行期间发生的情况，正如你在终端看到的那样：  
图片

为你的应用创建策略，GitLab会根据你的定义来运行pipeline。你的管道状态也会由GitLab显示：  
图片

最后，如果出现任何问题，可以轻松地回滚所有更改：  
图片

1.2. 基本 CI/CD 工作流程  
一旦你将提交推送到远程仓库的分支上，那么你为该项目设置的CI/CD管道将会被触发。GitLab CI/CD 通过这样做： 

运行自动化脚本（串行或并行） 代码Review并获得批准  
构建并测试你的应用  
就像在你本机中看到的那样，使用Review Apps预览每个合并请求的更改　  
代码Review并获得批准  
合并feature分支到默认分支，同时自动将此次更改部署到生产环境  
如果出现问题，可以轻松回滚  
通过GitLab UI所有的步骤都是可视化的  
图片

1.3. 深入了解CI/CD基本工作流程  
如果我们深入研究基本工作流程，则可以在DevOps生命周期的每个阶段看到GitLab中可用的功能，如下图所示：  
图片

  1. Verify  
    通过持续集成自动构建和测试你的应用程序  
    使用GitLab代码质量（GitLab Code Quality）分析你的源代码质量  
    通过浏览器性能测试（Browser Performance Testing）确定代码更改对性能的影响  
    执行一系列测试，比如Container Scanning , Dependency Scanning , JUnit tests  
    用Review Apps部署更改，以预览每个分支上的应用程序更改 

  2. Package  
    用Container Registry存储Docker镜像  
    用NPM Registry存储NPM包  
    用Maven Repository存储Maven artifacts  
    用Conan Repository存储Conan包 

  3. Release  
    持续部署，自动将你的应用程序部署到生产环境  
    持续交付，手动点击以将你的应用程序部署到生产环境  
    用GitLab Pages部署静态网站  
    仅将功能部署到一个Pod上，并让一定比例的用户群通过Canary Deployments访问临时部署的功能（PS：  
    即灰度发布）  
    在Feature Flags之后部署功能  
    用GitLab Releases将发布说明添加到任意Git tag  
    使用Deploy Boards查看在Kubernetes上运行的每个CI环境的当前运行状况和状态  
    使用Auto Deploy将应用程序部署到Kubernetes集群中的生产环境 

使用GitLab CI/CD，还可以：  
通过Auto DevOps轻松设置应用的整个生命周期  
将应用程序部署到不同的环境  
安装你自己的GitLab Runner  
Schedule pipelines  
使用安全测试报告（Security Test reports）检查应用程序漏洞 

GitLab CI/CD 快速开始  
.gitlab-ci.yml文件告诉GitLab Runner要做什么。一个简单的管道通常包括三个阶段：build、test、deploy  
管道在 CI/CD > Pipelines 页面

2.1. 创建一个 .gitlab-ci.yml 文件  
通过配置.gitlab-ci.yml文件来告诉CI要对你的项目做什么。它位于仓库的根目录下。

仓库一旦收到任何推送，GitLab将立即查找.gitlab-ci.yml文件，并根据文件的内容在Runner上启动作业。

下面是一个Ruby项目配置例子：  
image: "ruby:2.5"

before_script:

  * apt-get update -qq && apt-get install -y -qq sqlite3 libsqlite3-dev nodejs
  * ruby -v
  * which ruby
  * gem install bundler --no-document
  * bundle install --jobs $(nproc) "${FLAGS[@]}"
    
    rspec:  
    script:
    
      * bundle exec rspec
    
    rubocop:  
    script:
    
      * bundle exec rubocop  
        上面的例子中，定义里两个作业，分别是 rspec 和 rubocop，在每个作业开始执行前，要先执行before_script下的命令  
        2.2. 推送 .gitlab-ci.yml 到 GitLab  
        git add .gitlab-ci.yml  
        git commit -m "Add .gitlab-ci.yml"  
        git push origin master  
        2.3. 配置一个Runner  
        在GitLab中，Runner运行你定义在.gitlab-ci.yml中的作业（job）  
        一个Runner可以是一个虚拟机、物理机、docker容器，或者一个容器集群  
        GitLab与Runner之间通过API进行通信，因此只需要Runner所在的机器有网络并且可以访问GitLab服务器即可  
        你可以去 Settings ➔ CI/CD 看是否已经有Runner关联到你的项目，设置Runner简单又直接  
        图片
    
    2.4. 查看 pipeline 和 jobs的状态  
    在成功配置Runner以后，你应该可以看到你最近的提交的状态  
    图片

为了查看所有jobs，你可以去 Pipelines ➔ Jobs 页面  
图片

通过点击作业的状态，你可以看到作业运行的日志  
图片

回顾一下：  
1、首先，定义.gitlab-ci.yml文件。在这个文件中就定义了要执行的job和命令  
2、接着，将文件推送至远程仓库  
3、最后，配置Runner，用于运行job

<ol start="3">
  <li>
    Auto DevOps<br /> Auto DevOps 提供了预定义的CI/CD配置，使你可以自动检测，构建，测试，部署和监视应用程序。借助CI/CD最佳实践和工具，Auto DevOps旨在简化成熟和现代软件开发生命周期的设置和执行。
  </li>
</ol>

借助Auto DevOps，软件开发过程的设置变得更加容易，因为每个项目都可以使用最少的配置来完成从验证到监视的完整工作流程。只需推送你的代码，GitLab就会处理其他所有事情。这使得启动新项目更加容易，并使整个公司的应用程序设置方式保持一致。

下面这个例子展示了如何使用Auto DevOps将GitLab.com上托管的项目部署到Google Kubernetes Engine

示例中会使用GitLab原生的Kubernetes集成，因此不需要再单独手动创建Kubernetes集群

本例将创建并部署一个从GitLab模板创建的应用  
3.1. 从GitLab模板创建项目  
在创建Kubernetes集群并将其连接到GitLab项目之前，你需要一个Google Cloud Platform帐户  
下面使用GitLab的项目模板来创建一个新项目  
图片

给项目起一个名字，并确保它是公有的  
图片

3.2. 从GitLab模板创建Kubernetes集群  
点击 Add Kubernetes cluster 按钮，或者 Operations > Kubernetes  
图片

图片

图片

安装Helm, Ingress, 和 Prometheus  
图片

3.3. 启用Auto DevOps (可选)  
Auto DevOps 默认是启用的。  
导航栏 Settings > CI/CD > Auto DevOps  
勾选 Default to Auto DevOps pipeline  
最后选择部署策略  
图片

一旦你已经完成了以上所有的操作，那么一个新的 pipeline 将会被自动创建。为了查看pipeline，可以去 CI/CD > Pipelines  
图片

3.4. 部署应用  
到目前为止，你应该看到管道正在运行，但是它到底在运行什么呢？  
管道内部分为4个阶段，我们可以查看每个阶段有几个作业在运行，如下图：  
构建 -> 测试 -> 部署 -> 性能测试  
图片

现在，应用已经成功部署，让我们通过浏览器查看。  
首先，导航到 Operations > Environments  
图片

在Environments中，可以看到部署的应用的详细信息。在最右边有三个按钮，我们依次来看一下：  
第一个图标将打开在生产环境中部署的应用程序的URL。这是一个非常简单的页面，但重要的是它可以正常工作！  
紧挨着第二个是一个带小图像的图标，Prometheus将在其中收集有关Kubernetes集群以及应用程序如何影响它的数据（在内存/ CPU使用率，延迟等方面）  
图片

第三个图标是Web终端，它将在运行应用程序的容器内打开终端会话。 

<ol start="4">
  <li>
    <p>
      Examples<br /> 使用GitLab CI/CD部署一个Spring Boot应用<br /> 示例 .gitlab-ci.yml<br /> image: java:8
    </p>
    
    <p>
      stages:
    </p>
    
    <ul>
      <li>
        build
      </li>
      <li>
        deploy
      </li>
    </ul>
    
    <p>
      before_script:
    </p>
    
    <ul>
      <li>
        chmod +x mvnw
      </li>
    </ul>
    
    <p>
      build:<br /> stage: build<br /> script: ./mvnw package<br /> artifacts:<br /> paths:
    </p>
    
    <ul>
      <li>
        target/demo-0.0.1-SNAPSHOT.jar
      </li>
    </ul>
    
    <p>
      production:<br /> stage: deploy<br /> script:
    </p>
    
    <ul>
      <li>
        curl --location "<a href="https://cli.run.pivotal.io/stable?release=linux64-binary&source=github">https://cli.run.pivotal.io/stable?release=linux64-binary&source=github</a>" | tar zx
      </li>
      <li>
        ./cf login -u $CF_USERNAME -p $CF_PASSWORD -a api.run.pivotal.io
      </li>
      <li>
        ./cf push<br /> only:
      </li>
      <li>
        master
      </li>
    </ul>
  </li>
  
  <li>
    参考文档<br /> <a href="https://about.gitlab.com/solutions/kubernetes/">https://about.gitlab.com/solutions/kubernetes/</a><br /> <a href="https://docs.gitlab.com/ee/ci/README.html">https://docs.gitlab.com/ee/ci/README.html</a><br /> <a href="https://docs.gitlab.com/ee/ci/introduction/">https://docs.gitlab.com/ee/ci/introduction/</a><br /> <a href="https://docs.gitlab.com/ee/topics/autodevops/">https://docs.gitlab.com/ee/topics/autodevops/</a><br /> <a href="https://docs.gitlab.com/ee/ci/examples/README.html">https://docs.gitlab.com/ee/ci/examples/README.html</a>
  </li>
</ol>