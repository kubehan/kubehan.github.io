---
title: Kubernetes 两年使用经验总结
author: Kubehan
type: post
date: 2020-12-09T03:59:05+08:00
url: /3116.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/05/1600761460-99fdc41c82dc1ed.png
views:
  - 867
categories:
  - Linux运维

---
<section style="color: rgb(63, 63, 63);font-size: 14px;font-family: Avenir, -apple-system-font, 微软雅黑, sans-serif;text-align: center;box-sizing: border-box;" data-mpa-powered-by="yiban.io"> <section style="box-sizing: border-box;text-align: left;">来源：<span style="text-align: center;">https://lambda.grofers.com/learnings-from-two-years-of-kubernetes-in-production-b0ec21aa2814</span></section> <section style="box-sizing: border-box;text-align: left;"><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-c6d8a0f1d0a54290c908c411be9a61b8.png" /> </section> <section style="box-sizing: border-box;font-size: 15px;text-align: justify;line-height: 27px;color: rgb(89, 89, 89);background-color: rgb(239, 239, 239);padding: 19px;margin-top: 40px;margin-right: 8px;margin-left: 8px;"> 大约两年前，我们决定放弃在 EC2 平台中基于 Ansible 配置管理工具部署应用程序，并转向容器化和 Kubernetes 技术栈，使用 Kubernetes 进行应用程序编排。现在我们已将大部分基础架构迁移到 Kubernetes。这是一项艰巨的任务，也有它自己的挑战——从迁移进行之时运行混合基础设施的技术挑战，再到用全新的操作范式培训整个团队等等，不一而足。 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  在这篇文章中，我们将回顾我们的经验，并分享我们从这一过程中学到的东西，以帮助你做出更好的决定，增加你成功的机会。
</p><section style='box-sizing: border-box;font-size: 20px;color: rgb(0, 179, 139);text-align: left;line-height: 35px;margin-top: 40px;margin-right: 8px;margin-left: 8px;background-image: url("https://mmbiz.qpic.cn/mmbiz\_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPricJmiaibYVqJibVOAAVTSvOFiac7icPYWbkVv0shibrzq0GzZ2gNCzuzC8sRA/640?wx\_fmt=jpeg");background-position: left 28px;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 6px;'> 

<span style="display: block;font-size: 32px;margin-bottom: 10px;padding-left: 22px;">1</span> 迁移到 Kubernetes 的原因 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  无服务器技术和容器技术都很好，如果您要开始一项新业务并从头开始构建所有应用，请务必使用容器来部署应用程序。如果你拥有足够带宽（或可能不具备，请继续阅读），并拥有配置和操作 Kubernetes 以及将应用部署到 Kubernetes 的技术能力，请使用 Kubernetes 来编排你的容器化应用程序。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  即使您在 EKS、GKE 或 AKS 之类的托管平台上使用 Kubernetes，在其上正确部署和操作应用程序也具有一定的学习曲线。您的开发团队应该应对挑战。只有您的团队遵循 DevOps 理念，才能带来很多好处。如果您所处的情况是，由系统管理团队为其他团队开发的应用程序编写部署清单，那么从 DevOps 的角度来看，我个人认为 Kubernetes 能够带来的好处较小。当然，选择 Kubernetes 可以为您带来许多其他好处，例如更低的成本、更快的实验、更快的自动缩放、弹性等。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  如果你已经在云平台虚拟机或其他 PaaS 平台上部署应用，那么你真的要考虑从现有的基础设施迁移到 Kubernetes 吗？你确信 Kubernetes 是解决你的问题的唯一途径吗？你必须清楚你的动机，因为将现有的基础设施迁移到 Kubernetes 是一项艰巨的任务。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们在这方面犯了一些错误。我们迁移到 Kubernetes 的主要原因是为了建立一个持续集成的基础架构，该基础架构可以帮助我们快速重构微服务。这些年来，这些微服务在架构方面逐渐积累了很多问题，因而需要重构。大多数新功能都需要修改多个代码库，因此，同时开发和测试所有这些微服务的工作会拖累我们的速度。我们认为有必要为每个开发人员和每个变更提供一个集成环境，以帮助加快开发和测试周期，而无需协调谁来获得“共享预发布环境”。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-2bc8f2d739552f68b681ace0b77c3cb3.png" />
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们的持续集成流水线之一，可为所有微服务提供新的集成环境并运行自动化测试
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们现在做得很好。今天我们可以在 8 分钟内在 Kubernetes 上的集成环境中部署 21 个微服务。任何开发人员都可以使用我们自己开发的工具来执行此操作。我们还为这 21 个微服务中的任何一个创建的拉取请求都提供了这个环境的子集。整个测试周期（提供环境和运行测试）需要不到 12 分钟的时间。这可能感觉很长，但它可以防止我们在当前混乱的架构中交付糟糕的更改。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: center;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-44e07b4b7ba7ca0e98d25f4ed278c897.png" />持续集成流水线执行报告
</p><section style='box-sizing: border-box;font-size: 20px;color: rgb(0, 179, 139);text-align: left;line-height: 35px;margin-top: 40px;margin-right: 8px;margin-left: 8px;background-image: url("https://mmbiz.qpic.cn/mmbiz\_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPricJmiaibYVqJibVOAAVTSvOFiac7icPYWbkVv0shibrzq0GzZ2gNCzuzC8sRA/640?wx\_fmt=jpeg");background-position: left 28px;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 6px;'> 

<span style="display: block;font-size: 32px;margin-bottom: 10px;padding-left: 22px;">2</span> Kubernetes 的迁移方式 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们花了将近一年半的时间让这一复杂的持续集成流水线稳定运行，我们构建了额外工具，创建遥测工具并重新配置了每个应用程序的部署方法。为了开发和生产环境的一致性，我们也必须将所有这些微服务部署到生产环境中，否则基础设施和部署应用之间的偏离将使应用程序难以为开发人员所理解，并将使开发人员的运维成为一场噩梦。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们对这个话题有复杂的感觉。回顾过去，我们认为我们解决持续集成的方式使问题变得更糟了，因为将所有微服务推向生产环境（为了开发 / 生产环境一致性）的复杂性使得实现更快的持续集成构建这一目标变得更加复杂和困难。在使用 Kubernetes 之前，我们使用 Ansible 和 Hashicorp Consul 以及 Vault 来提供基础设施、配置管理和部署。它们是缓慢的吗? 当然是。但我们认为，我们本可以在 Consul 中引入服务发现，并对 Ansible 部署进行一些优化，这样我们可以在相当短的时间内接近我们的目标。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们应该迁移到 Kubernetes 吗？答案当然是肯定的。使用 Kubernetes 有几个好处，包括服务发现、更好的成本管理、弹性、治理、云基础设施的基础设施抽象等等。我们今天也获得了所有这些好处。但这并不是我们开始时的主要目标，而且实现这些的过程中所产生的压力和痛苦也许是不必要的。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  对我们来说，一个重要的教训是，我们本可以选择一条不同的、阻力较小的道路来采用 Kubernetes。我们只是把 Kubernetes 作为唯一的解决方案，我们甚至没有评估其他选择。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们将在这篇文章中看到，将应用迁移到 Kubernetes 并在其上操作与将应用部署在云平台虚拟机或裸机上是不同的。<strong style="text-align: left;">对于您的云工程师和开发团队来说，这会有一个学习曲线</strong>。让您的团队学习这些技术可能是值得的。但问题是，你现在需要这么做吗？你必须尽量清楚的回答这一问题。
</p><section style='box-sizing: border-box;font-size: 20px;color: rgb(0, 179, 139);text-align: left;line-height: 35px;margin-top: 40px;margin-right: 8px;margin-left: 8px;background-image: url("https://mmbiz.qpic.cn/mmbiz\_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPricJmiaibYVqJibVOAAVTSvOFiac7icPYWbkVv0shibrzq0GzZ2gNCzuzC8sRA/640?wx\_fmt=jpeg");background-position: left 28px;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 6px;'> 

<span style="display: block;font-size: 32px;margin-bottom: 10px;padding-left: 22px;">3</span> 开箱即用的 Kubernetes 还远远不够 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  很多人错误的认为 Kubernetes 是一种 PaaS 解决方案，事实并非如此，Kubernetes 是一个用于构建 PaaS 解决方案的平台。OpenShift 就是这样一个例子。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  对几乎所有人来说，开箱即用的 Kubernetes 都远远不够。Kubernetes 平台是一个学习和探索的好地方。但是您很可能需要更多基础设施组件，并将它们很好地结合在一起作为应用程序的解决方案，以使其对开发人员更有意义。通常，这一套带有额外基础设施组件和策略的 Kubernetes 被称为内部 Kubernetes 平台。这是一个非常有用的使用范例，有几种方法可以扩展 Kubernetes。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  指标、日志、服务发现、分布式追踪、配置和 secret 管理、持续集成 / 持续部署、本地开发体验、根据自定义指标自动扩展都是需要关注和做出决策的问题。这些只是我们指出的一部分事情。肯定还有更多的决策需要制定，有更多的基础设施需要建立。一个重要的领域是你的开发人员将如何使用 Kubernetes 资源和清单文件，这篇文章的后面会有更多这方面的内容。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  以下是我们的一些决策及其理由。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
指标 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们最后选择使用 Prometheus。现在 Prometheus 几乎是一个事实上的指标基础设施。CNCF 和 Kubernetes 对它非常友好。它在 Grafana 生态系统中工作得很好，我们很喜欢 Grafana！唯一的问题是我们以前使用 InfluxDB。我们已经决定抛弃 InfluxDB，并全力转向 Prometheus。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
日志 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  对我们来说，日志一直是一个大问题。我们已经努力使用 ELK 构建了一个稳定的日志平台。我们发现 ELK 有很多我们团队用不到的功能，而这些功能是有代价的。同时，我们认为使用 Elasticsearch 处理日志是一个昂贵的解决方案，因而也有潜在的风险。我们最后决定使用 Grafana 的 Loki。它很简单，而且具有我们团队所需的必要功能。这是非常符合成本效益的。但最重要的是，由于它的查询语言非常类似于 PromQL，所以它具有优越的用户体验。而且，它对 Grafana 非常友好，这样就可以在一个用户界面中集成指标监控和日志记录。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-bb9d03274fee61ddba05217000f8c705.png" />
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  这是一个 Grafana 仪表盘的例子，它可以同时可视化指标和相应的日志
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
配置和 Secret 管理 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  你会发现大多 Kubernetes 项目都使用了 configmap 和 secret 对象。我们意识到 configmap 和 secret 可以作为起点，但却不足以满足我们的应用场景。对现有服务使用 configmap 有一定代价。Configmap 可以通过某种方式装载到 pods 中，使用该方法配置环境变量是最常见的方式。如果您有大量的遗留微服务从配置管理工具（如 Puppet、Chef 或 Ansible）提供的文件中读取配置，那么您将不得不在所有代码库中重做配置处理，让 configmap 从环境变量中读取配置。我们没有找到足够的理由去做这件事。另外，配置或 secret 的更改意味着您必须重新部署应用使其生效。这将需要使用额外的 kubectl 命令来完成。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-acbab4fecd86fd8d411751e54cc1602f.png" />
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  为了避免这一切，我们决定使用 Consul、Vault 和 Consul 模板进行配置管理。现在我们将 Consul 模板作为初始化容器（init container）运行，并计划将其作为 pod 中的旁路（side car）容器，以便它能够监听 Consul 中的配置更改，从 Vault 刷新过期的 secret，并优雅地重新加载应用程序进程。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
持续集成和持续部署 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  在迁移到 Kubernetes 之前，我们使用的是 Jenkins。迁移到 Kubernetes 后，我们决定继续使用 Jenkins。到目前为止，我们的经验是 Jenkins 并不是使用云原生基础设施的最佳解决方案。我们发现为了实现目标，我们需要使用 Python、Bash、Docker 和脚本化或声明式 Jenkins 流水线做了很多探究工作。构建和维护这些工具和流水线一开始十分困难。我们现在正在探索使用 Tekton 和 Argo Workflows 作为我们新的 CI/CD 平台。您可以在 CI/CD 工具箱中探索更多选项，如 Jenkins X、Screwdriver、Keptn 等。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
部署经验 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  在部署工作流程中有许多使用 Kubernetes 的方法。我们主要将其归结为两种选择，分别是 Telepresence.io 和 Skaffold。Skaffold 能够监视您的本地更改，并将它们持续部署到 Kubernetes 集群中。而 Telepresence 则允许您在本地运行服务，同时在 Kubernetes 集群中设置透明的网络代理，这样您的本地服务就可以与 Kubernetes 中的其他服务通信，就像它部署在集群中一样。选择哪种工具是个人的意见和喜好问题。确定使用某一种工具很困难，我们现在主要在尝试使用 Telepresence，但 Skaffold 可能成为我们更好的工具，我们没有放弃这种可能性。只有时间能证明我们最终使用哪个工具，又或者两者都用。此外也还有其他的解决方案，例如 Draft 就是一个值得关注的选项。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
分布式追踪 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们目前还没有做分布式跟踪。不过，我们计划很快在这个领域进行投入。就像日志记录一样，我们希望在 Grafana 的指标和日志记录旁添加分布式跟踪，从而为我们的开发团队提供一个更集中的可观测性体验。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
应用打包、部署和相关工具 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  Kubernetes 中很重要的一个方面是考虑开发人员如何与集群交互并部署他们的工作负载。我们想让事情保持简单并易于扩展。我们正在尝试使用 Kustomize、Skaffold 以及一些自主开发的 CRD 来作为开发人员部署和管理应用程序的工具。尽管如此，任何团队都可以自由地使用他们想要使用的任何工具来与集群交互，只要这些工具是开源的并建立在开放标准之上。
</p><section style='box-sizing: border-box;font-size: 20px;color: rgb(0, 179, 139);text-align: left;line-height: 35px;margin-top: 40px;margin-right: 8px;margin-left: 8px;background-image: url("https://mmbiz.qpic.cn/mmbiz\_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPricJmiaibYVqJibVOAAVTSvOFiac7icPYWbkVv0shibrzq0GzZ2gNCzuzC8sRA/640?wx\_fmt=jpeg");background-position: left 28px;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 6px;'> 

<span style="display: block;font-size: 32px;margin-bottom: 10px;padding-left: 22px;">4</span> 操作 Kubernetes 集群并不容易 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们主要使用 AWS 的新加坡区域。当我们开始使用 Kubernetes 时，在新加坡区域还不能使用 EKS 服务。因此，我们必须使用 kops 在 EC2 上建立自己的 Kubernetes 集群。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  配置一个基础的集群可能并不困难。我们在一周内就建立起了第一个集群，而大多数问题发生在我们开始部署工作负载时。从调整集群自动伸缩器（autoscaler）到在正确的时间配置资源，再到正确配置网络以实现所需的性能，你都必须自己研究和配置。在大多数情况下，默认配置在生产环境中大部分时候都不能正常工作（或者至少在那时无法在我们的场景中正常工作）。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们的学习到的是，操作 Kubernetes 是很复杂的。它有很多活动部件。而学习如何操作 Kubernetes 很可能不是你业务的核心。尽可能多地将这些工作卸载给云服务提供商 (EKS、GKE、AKS)。你自己做这些事并不会带来价值。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
考虑升级 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  即使您使用的是托管服务，Kubernetes 也非常复杂，升级也不会一帆风顺。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  即使在使用托管的 Kubernetes 服务时，也要尽早进行基础设施即代码的设置，以使灾难恢复和升级过程在未来相对不那么痛苦，并且能够在灾难发生时快速恢复。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  如果你愿意，你可以试着使用 GitOps。如果你不能做到这一点，那么将手动步骤减少到最低限度是一个很好的开始。我们结合使用 eksctl、terraform 和我们的集群配置清单（包括平台服务清单）来建立我们所称的“Grofers Kubernetes 平台”。为了使设置和部署过程更简单和可重复，我们构建了一个自动化流水线来设置新的集群并将更改部署到现有的集群。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
资源需求和限制 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  在开始迁移之后，我们发现由于不正确的配置，集群中出现了许多性能和功能问题。为了解决这些问题，我们添加了很多资源请求和限制，以消除可能导致性能下降的资源约束。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  最初的观察结果之一是由于节点的内存限制而导致的 pod 驱逐。其原因是与资源请求相比，资源限制过高。随着流量的激增，内存消耗的增加可能导致节点上的内存耗尽，并进一步导致 pod 被驱逐。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们学习到应该保持资源请求足够高但不太高，这样在低流量时我们会浪费一些资源。同时保持资源限制相对接近资源请求，以便为峰值流量留出一些喘息的空间，而不会由于节点的内存压力而导致 pod 被驱逐。限制与请求的距离有多近取决于你的流量模式。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  这不适用于非生产环境（如开发、预发布和持续集成），因为这些环境不会出现任何流量高峰。理论上，如果将容器的 CPU 请求设置为零并设置足够高的 CPU 限制，就可以运行无限个容器。如果您的容器开始使用大量的 CPU，它们将被限制性能。您也可以对内存请求和限制执行同样的操作。然而，应用达到内存限制后的情形与 CPU 不同。如果您使用的内存超过了设置的限制，那么您的容器会因为内存耗尽（OOM）而被杀死并重新启动。如果内存限制异常高 (比方说超过节点的容量)，则可以继续使用内存，但最终当节点用完可用内存时，调度器将开始驱逐 pod。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  在非生产环境中，我们通过保持极低的资源请求和极高的资源限制来尽可能安全地分配资源。这种情况下的限制因素是内存，也就是说，无论内存请求有多低，内存限制有多高，pod 驱逐都由调度在节点上的所有容器所使用的内存总和所决定。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
安全与治理 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  Kubernetes 旨在让开发者使用云平台，使他们更加独立，并推动 DevOps 文化。向开发人员开放平台、减少云工程团队（或系统管理员）的干预以及使开发团队独立应该是重要目标之一。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  有时这种独立性会带来严重的风险。例如，在 EKS 中使用 LoadBalancer 类型的 service 会在缺省情况下提供面向公共网络的 ELB，而添加某个注释将确保提供内部 ELB。我们在刚开始在这个问题上犯了一些错误。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们使用 Open Policy Agent 来降低各种安全风险，以及与成本、安全和技术债务相关的风险。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  部署 Open Policy Agent 来构建正确的控制器有助于自动化整个变更管理过程，并为我们的开发人员正确构建安全的网络。使用 Open Policy Agent，我们可以像前面提到的那样设置限制——除非有正确的注释，否则限制服务对象的创建，这样开发人员就不会意外地创建公共的 ELB。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
成本 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们在迁移后看到了巨大的成本效益。然而，并不是所有的好处都立竿见影。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  注意：我们正在整合一个关于我们最近成本优化计划的更详细的帖子，请关注我们的 Lambda。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
更好的资源利用能力 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  这是最明显的一个好处。我们今天的基础设施所提供的计算、内存和存储比以前少得多。除了由于更好的容器或进程包装而获得更高的资源利用率之外，我们还能够比以前更好地利用共享服务，如可观察性（指标、日志）组件。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  然而，在迁移过程中我们浪费了大量的资源。由于我们无法很快优化我们的自我管理 Kubernetes 集群，我们遇到了大量的性能问题。我们为我们的 pod 请求大量资源以作为缓冲，这更像是提供一种保证，以减少停机和由于缺乏计算或内存资源所导致的性能问题。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  由较高的资源缓冲而导致的高基础设施成本是一个大问题。使用 Kubernetes 本应让我们获得较高的资源利用率，但我们并没有真的获得这种好处。在迁移到 EKS 之后，它带来的稳定性帮助我们变得更加自信，这帮助我们采取必要的步骤来纠正资源请求，并大幅降低资源浪费。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
Spot </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  在 Kubernetes 中使用 spot 实例要比在普通虚拟机中容易得多。使用虚拟机，您可以自己管理 spot 实例，这可能比较复杂，如需要确保应用程序的正常运行时间或使用像 SpotInst 这样的服务。这同样适用于 Kubernetes，但是 Kubernetes 带来的较高资源利用效率可以为您保留足够的空间来做缓冲，这样即使集群中的一些实例中断，运行于其上的容器也可以快速地在其他地方重新调度。有几个选项可以有效地管理 spot 中断。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  Spot 实例帮助我们节省了大量资金。今天，我们的整个预发布 Kubernetes 集群运行在 spot 实例上，99% 的生产 Kubernetes 集群由保留实例、节约计划和 spot 实例覆盖。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  对我们来说，优化的下一步是如何在 spot 实例上运行整个生产集群。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
ELB 整合 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们使用 Ingress 来整合我们的预发布环境中的 ELB，这大幅降低了 ELBs 的固定成本。为了避免这造成生产环境和部署环境代码的差异，我们决定实现一个 controller，该 controller 将 LoadBalancer 类型服务更改为 NodePort 类型服务，同时在我们的预发布集群中创建一个 ingress 对象。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  迁移到 Nginx ingress 对我们来说相对简单，因为我们采用了控制器方法，所以不需要做太多更改。如果我们在生产环境中也使用 ingress，更改则会更少。在生产环境使用 ingress 不是一个简单的改变。如要正确的为生产环境配置 ingress，就必须考虑周详，还需要从安全性和 API 管理的角度来考虑。这是我们打算在不久的将来开展工作的一个领域。
</p><section style="box-sizing: border-box;font-size: 16px;text-align: left;margin-top: 30px;margin-left: 8px;color: rgb(0, 179, 138);">

<span style='display: inline-block;width: 15px;height: 15px;background-image: url("https://mmbiz.qpic.cn/mmbiz_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPrPEoFkh6Ty0E0oicEZ9Pia9FC64pQqC5snGiaKibqiaJYE0qbic08CKoOopMA/640?wx_fmt=jpeg");background-position: center center;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 100%;margin-right: 10px;'></span>  
增加跨 AZ 数据传输 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  虽然我们在基础设施方面节省了大量开支，但在基础设施的另一个领域，即跨 AZ 数据传输方面，我们的成本却在增加。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  Pod 可以被调度到任何节点上。即使您控制了 pod 在集群中的调度方式，也没有简单的方法来控制服务如何发现彼此（即一个服务的 pod 与同一 AZ 中的另一个服务的 pod 通信），以减少跨 AZ 的数据传输。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  经过大量的研究和与其他公司的同行的交谈，我们了解到可以通过引入一个服务网格来控制从一个 pod 路由到目的地 pod 的流量来实现这一功能。操作服务网格很复杂，我们还没有准备好为了节省跨 AZ 数据传输的成本而自己承担这一复杂性。
</p><section style='box-sizing: border-box;font-size: 20px;color: rgb(0, 179, 139);text-align: left;line-height: 35px;margin-top: 40px;margin-right: 8px;margin-left: 8px;background-image: url("https://mmbiz.qpic.cn/mmbiz\_jpg/FE4VibF0SjfOQakOc5Wef6GyIDB3BRbPricJmiaibYVqJibVOAAVTSvOFiac7icPYWbkVv0shibrzq0GzZ2gNCzuzC8sRA/640?wx\_fmt=jpeg");background-position: left 28px;background-repeat: no-repeat;background-attachment: initial;background-origin: initial;background-clip: initial;background-size: 100% 6px;'> 

<span style="display: block;font-size: 32px;margin-bottom: 10px;padding-left: 22px;">5</span> CRDs、Operators 和 Controllers：简化运维和更完整的体验 </section> 

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  每个组织都有自己的工作流程和运营挑战，我们也不例外。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  在对 Kubernetes 的两年探索中，我们了解到 Kubernetes 很棒，但如果你使用它的 controllers、operators 和 CRDs 等功能来简化日常操作，并为开发者提供更完整的体验，那就更好了。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们已经开始开发一系列 controllers 和 CRDs。例如，LoadBalancer 类型 service 到 ingress 的转换是一个 controller 操作。类似地，每当部署新服务时，我们使用 controller 在 DNS 中自动创建 CNAME 记录。我们有另外的 5 个独立用例，他们依赖我们的内部 controller 来简化日常操作和减少苦活累活。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们还建立了一些 CRD。其中一种被广泛用于在 Grafana 上生成监视面板，它声明式地指定监视面板应该由什么构成。这使得开发人员可以在他们的应用代码库旁配置他们的监控面板，并使用相同的命令（kubectl apply -f .）部署一切内容。
</p>

<p style="box-sizing: border-box;font-size: 16px;text-align: justify;white-space: pre-line;line-height: 27px;padding-top: 23px;padding-right: 8px;padding-left: 8px;color: rgb(74, 74, 74);">
  我们正在看到 controller 和 CRD 的大量好处。随着我们与云计算供应商 AWS 紧密合作以简化集群基础设施操作，我们将自己解放出来，更多地专注于构建“Grofers Kubernetes 平台”，该平台被设计为尽力以最好的方式支持我们的开发团队。
</p>

<p style='padding-right: 0.5em;padding-left: 0.5em;white-space: normal;letter-spacing: 0.544px;color: rgb(0, 0, 0);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif;font-size: 16px;text-align: center;'>
  <span style="color: rgb(2, 30, 170);font-family: monospace;letter-spacing: 0.544px;white-space: pre-wrap;background-color: rgb(255, 255, 255);">- END -</span>
</p>

<p style='padding-right: 0.5em;padding-left: 0.5em;white-space: normal;letter-spacing: 0.544px;color: rgb(0, 0, 0);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, "PingFang SC", Cambria, Cochin, Georgia, Times, "Times New Roman", serif;font-size: 16px;text-align: center;'>
</p><section data-mpa-template-id="167209" data-mpa-color="#ffffff" data-mpa-category="body" style="white-space: normal;font-size: 14px;letter-spacing: 0.544px;text-align: left;font-family: monospace;background-color: rgb(255, 255, 255);"> 

<pre style="letter-spacing: 0.544px;"><pre><pre style="color: rgb(0, 0, 0);letter-spacing: 0.544px;"><section data-mpa-template-id="167209" data-mpa-color="#ffffff" data-mpa-category="body" style="white-space: normal;letter-spacing: 0.544px;">

<pre style="letter-spacing: 0.544px;"><pre>&lt;section style='font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><span style="transform: translate3d(0px, 0px, 1px);display: inline-table;transform-style: preserve-3d;text-indent: unset;"><span style="transform: translate3d(0px, 0px, 1px);display: inline-block;"> </span><span style="font-size: 16px;transform: translate3d(0px, 0px, 1px);display: inline-block;font-weight: bold;">推荐阅读 </span><span data-mpa-emphasize-underline-bg-line="t" style="margin-top: -10px;font-size: 16px;width: 77px;height: 10px;background-color: rgb(250, 168, 68);display: block;transform: translate3d(0px, -4px, 0px);"><br /><br /></span></span>&lt;/section></pre>
</section>
<section data-mpa-template-id="167209" data-mpa-color="#ffffff" data-mpa-category="body" style="white-space: normal;letter-spacing: 0.544px;">


<pre style="letter-spacing: 0.544px;"><pre><pre style="letter-spacing: 0.544px;">&lt;section style='color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzAwNTM5Njk3Mw==&mid=2247495965&idx=1&sn=05c7e11a6cd049ddaef0033cd7f6c216&chksm=9b1ff19fac687889d1df5510e3133f2f11918a95625972f293be5778b1f07d2b86a15d1f3f22&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" data-linktype="2" style="text-decoration: underline;" rel="noopener noreferrer">Shell中各种括号的作用：()、(())、[]、[[]]、{}</a>&lt;/section>&lt;section style='color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzAwNTM5Njk3Mw==&mid=2247495952&idx=1&sn=033e608d4b5d7631395dab7a620b9d8b&chksm=9b1ff192ac687884c2e4c2026bec87d5afe717b3882e48ef3fb019793abe6418fd3e1699d965&scene=21#wechat_redirect" data-itemshowtype="5" tab="innerlink" data-linktype="2" style="text-decoration: underline;" rel="noopener noreferrer">有了Docker，为什么还用Kubernetes？</a>&lt;/section>&lt;section style='color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzAwNTM5Njk3Mw==&mid=2247495948&idx=1&sn=b50876026765e0441e9c8f68d6b94ad0&chksm=9b1ff18eac687898e68fc935151b24f6a3af77dedcf69253fe3985c434e04ec6924b759de6ee&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" data-linktype="2" style="text-decoration: underline;" rel="noopener noreferrer">在Kubernetes 1.20版本中不推荐使用Docker ！</a><br />&lt;/section>&lt;section style='color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzAwNTM5Njk3Mw==&mid=2247495880&idx=1&sn=c36700ba54390a936a38d11118377b54&chksm=9b1ff04aac68795cb86f2f009626af3acedac9d88286e6dbb1485acc00fe17f7ccb5769f8666&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" data-linktype="2" style="text-decoration: underline;letter-spacing: 0.544px;" rel="noopener noreferrer">Linux 运维必备的 40 个命令总结</a>&lt;/section>&lt;section style='color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzAwNTM5Njk3Mw==&mid=2247495837&idx=1&sn=a8b281d9f6356b75b5c406c2e7e24890&chksm=9b1ff01fac6879097582fd2a41b136669eb940ae1ca36d31505e9a12ad380ed97aa6e5e61b17&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" data-linktype="2" style="text-decoration: underline;letter-spacing: 0.544px;" rel="noopener noreferrer">系统架构性能优化思路</a>&lt;/section>&lt;section style='color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzAwNTM5Njk3Mw==&mid=2247495798&idx=1&sn=762aa7cadb8276ca1e6abafa0a149027&chksm=9b1ff0f4ac6879e2a1d86430114c5aea4e4c28848579cb17997ea1bfec29cfdfd14b49de095d&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" data-linktype="2" style="text-decoration: underline;" rel="noopener noreferrer">Kubernetes 的这些原理，你一定要了解</a>&lt;/section>&lt;section style='color: rgb(0, 0, 0);font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;line-height: 2em;'><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzAwNTM5Njk3Mw==&mid=2247495068&idx=1&sn=585db2088cfb062907493d0d6d0239ae&chksm=9b1fed1eac6864085249762523411888c43302654eaf72285d27a88acbfd853981f549010cb1&scene=21#wechat_redirect" data-itemshowtype="0" tab="innerlink" data-linktype="2" style="text-decoration: underline;" rel="noopener noreferrer">在项目实践中，进行了以下DevOps方案建设</a>&lt;/section></pre>
</section>


<p style='padding-right: 0.5em;padding-left: 0.5em;color: rgb(0, 0, 0);font-size: 16px;font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;text-align: center;'>
  
</p>
<section data-mpa-template-id="167209" data-mpa-color="#ffffff" data-mpa-category="body" style="color: rgb(0, 0, 0);font-size: 16px;white-space: normal;letter-spacing: 0.544px;">


<pre style="letter-spacing: 0.544px;"><p style='padding-right: 0.5em;padding-left: 0.5em;font-family: -apple-system-font, BlinkMacSystemFont, "Helvetica Neue", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei UI", "Microsoft YaHei", Arial, sans-serif;letter-spacing: 0.544px;white-space: normal;text-align: center;'>
  <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-79b19a7315e119b826491306029ac573.jpeg" />
</p></pre>
</section>


<p style="color: rgb(0, 0, 0);font-size: 16px;white-space: normal;">
  
</p>


<p style="color: rgb(0, 0, 0);white-space: normal;letter-spacing: 0.544px;text-align: right;">
  <span style="font-family: tahoma, helvetica, arial, sans-serif;letter-spacing: 0.544px;color: rgb(255, 104, 39);">点亮，服务器三年不宕机</span><img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/12/frc-17429b1d84cf45ba140d5e3f926da1dc.gif" />
</p>
</section>
</section>