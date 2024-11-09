---
title: Docker，Kubernetes在DevOps中的作用
author: Kubehan
type: post
date: 2020-05-07T10:41:24+08:00
url: /2475.html
Baidusubmit:
  - 1
views:
  - 811
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/05/frc-d944a70b9cd3d0a0cb6a9e5fb723db4e.png
categories:
  - Linux运维

---
<h2 style='margin-top: 18px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-size: 18px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);border-bottom: 1px solid rgb(234, 234, 234);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  大纲
</h2>

<blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
  <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
    <li style="margin-bottom: 6px;font-size: 12px;">
      DevOps是什么？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      为什么我们需要DevOps？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      DevOps与敏捷开发有何不同？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      重要的DevOps工具是什么？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      Docker如何帮助DevOps？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      Kubernetes如何帮助DevOps？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      Azure DevOps如何帮助DevOps？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      持续集成，持续交付是什么？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      基础架构即代码是什么？</p>
    </li>
    <li style="margin-bottom: 6px;font-size: 12px;">
      Terraform和Ansible如何帮助DevOps？</p>
    </li>
  </ul>
</blockquote>

<h2 style='margin-top: 18px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-size: 18px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);border-bottom: 1px solid rgb(234, 234, 234);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  DevOps是什么？
</h2>

<p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  与围绕软件开发的大多数流行语一样，DevOps没有公认的定义。
</p>

<p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  定义从简单（如这两个定义）到复杂（可以持续一整本书）不等。
</p>

<blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
  <p style="margin-bottom: 3px;font-size: 12px;">
    DevOps是文化理念，实践和应用快速交付工具的组合–AWS
  </p>
</blockquote>

<blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
  <p style="margin-bottom: 3px;font-size: 12px;">
    DevOps是组织内部的跨学科协作，旨在确保软件新版本的自动化持续交付，同时确保其正确性和可靠性 –L Leite
  </p>
</blockquote>

<p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  与其尝试定义DevOps，不如让我们了解软件开发如何演变为DevOps。
</p>

<h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  瀑布模型(Waterfall)
</h3>

<p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  几十年前，软件开发以Waterfall模型为中心。
</p>

<p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  Waterfall模型开发，与建筑项目类似-例如，建立一座令人惊叹的桥梁。
</p>

<p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  你将分多个阶段构建软件，这些阶段可以持续几周到几个月不等。
</p>

<p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
    在大多数Waterfall项目中，企业要几个月才能看到应用程序的有效版本。
  </p>
  
  <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
    构建优秀软件的关键要素
  </h3>
  
  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
    在瀑布模型中工作了几十年，我了解到了开发出色软件的一些关键要素：
  </p>
  
  <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
    <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
      <li style="margin-bottom: 6px;font-size: 12px;">
        沟通</p>
      </li>
      <li style="margin-bottom: 6px;font-size: 12px;">
        反馈</p>
      </li>
      <li style="margin-bottom: 6px;font-size: 12px;">
        自动化</p>
      </li>
    </ul>
  </blockquote>
  
  <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
    沟通的重要性
  </h4>
  
  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
    软件开发是一项涉及多种技能的跨学科工作。
  </p>
  
  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      人与人之间的交流对于软件项目的成功至关重要。
    </p>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      在瀑布模型中，我们尝试通过准备有关需求，设计，体系结构和部署的详细文档来增强交流。
    </p>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      但是，一段时间以来，我们了解到
    </p>
    
    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
        <li style="margin-bottom: 6px;font-size: 12px;">
          增强团队内部沟通的最佳方法–使团队团结在一起，这样就能在同一个团队中获得各种技能。</p>
        </li>
        <li style="margin-bottom: 6px;font-size: 12px;">
          具有多种技能的跨职能团队，会使工作出色。</p>
        </li>
      </ul>
    </blockquote>
    
    <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      早期反馈的重要性
    </h4>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      快速获得反馈很重要。开发出色的软件就是要获得快速反馈。
    </p>
    
    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
      <p style="margin-bottom: 3px;font-size: 12px;">
        我们是否正在构建符合业务期望的应用程序？
      </p>
    </blockquote>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      你不能等待几个月才能获得反馈。你可能想快速了解。
    </p>
    
    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
      <p style="margin-bottom: 3px;font-size: 12px;">
        如果将其部署到生产环境中，你的应用程序会出现问题吗？
      </p>
    </blockquote>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      我们越早发现问题，越容易解决。
    </p>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      我们发现，最好的软件团队通常是快速反馈。帮助我们知道是否尽早在做正确的事情。
    </p>
    
    <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      自动化的重要性
    </h4>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      自动化至关重要。软件开发涉及广泛的活动。手动执行操作速度很慢且容易出错。
    </p>
    
    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
        在了解了开发出色软件的关键要素之后，让我们看看我们如何演变为敏捷开发和DevOps。
      </p>
      
      <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
        敏捷开发发展
      </h3>
      
      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
        如果通过加强团队之间的沟通，获取快速反馈和引入自动化，敏捷开发是第一步。
      </p>
      
      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
          敏捷开发将业务团队和开发团队整合为一个团队，该团队致力于以小型迭代（称为Sprints）构建出色的软件。
        </p>
        
        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
            敏捷开发关注开发周期不是在数周或数，而是在几天甚至一天之内的小需求。
          </p>
          
          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
            <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              敏捷开发如何增强团队之间的沟通？
            </h4>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              敏捷开发使业务团队和开发团队聚集在一起。
            </p>
            
            <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
              <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                <li style="margin-bottom: 6px;font-size: 12px;">
                  业务团队负责定义构建什么内容。有什么要求？</p>
                </li>
                <li style="margin-bottom: 6px;font-size: 12px;">
                  开发团队负责构建满足要求的产品。开发包括与你的软件的设计，编码，测试和打包有关的每个人。</p>
                </li>
              </ul>
            </blockquote>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              在敏捷开发中，业务代表（通常称为产品负责人）经常出现在团队中，团队可以清楚地了解业务目标。
            </p>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              当开发团队对需求的理解不正确并且走错了路时，产品负责人将帮助他们进行课程更正，并保持正确的道路。
            </p>
            
            <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
              <p style="margin-bottom: 3px;font-size: 12px;">
                结果：团队构建的最终产品是企业想要的东西。
              </p>
            </blockquote>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              另一个重要因素，是敏捷开发团队具有跨职能的技能：编码技能（前端，API和数据库），测试技能和业务技能。
            </p>
            
            <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              敏捷开发与自动化
            </h4>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              敏捷开发团队关注哪些自动化领域？
            </p>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              软件产品可能具有多种缺陷：
            </p>
            
            <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
              <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                <li style="margin-bottom: 6px;font-size: 12px;">
                  功能缺陷意味着产品无法正常工作。</p>
                </li>
                <li style="margin-bottom: 6px;font-size: 12px;">
                  技术缺陷使软件维护困难。例如，代码质量问题。</p>
                </li>
              </ul>
            </blockquote>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              通常，敏捷开发团队专注于使用自动化来尽早发现技术和功能缺陷。
            </p>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              敏捷开发团队专注于自动化测试。编写出色的单元测试以测试你的方法和类。编写出色的集成测试以测试你的模块和应用程序。敏捷开发团队还广泛关注代码质量–使用诸如SONAR之类的工具来评估应用程序的代码质量。
            </p>
            
            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                如果你拥有出色的自动化测试和出色的代码质量检查，是否就足够了？
              </p>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                你可能还希望尽可能经常地运行它们。敏捷开发团队专注于持续集成。提交代码，运行单元测试，自动化测试和代码质量检查。这些都是在持续集成管道中自动执行的。在敏捷开发早期，最受欢迎的CI/CD工具是Jenkins。
              </p>
              
              <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                敏捷开发如何促进即时反馈？
              </h4>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                最重要的因素是，企业无需等待数月即可看到最终产品。在每次版本迭代结束时，都会将该产品演示给所有利益相关者。在优先考虑下一次版本迭代结束时，将采纳所有反馈。结果：团队构建的最终产品是企业想要的东西。
              </p>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                能够立即反馈的另一个重要因素是–持续集成。假设我将一些代码提交到版本控制中，在30分钟内，如果我的代码导致单元测试失败或集成测试失败，我将获得反馈。如果我的代码不符合代码质量标准或在单元测试中没有足够的代码覆盖率，我将获得反馈。
              </p>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                敏捷开发成功了吗？是。当然。通过专注于改善业务与开发团队之间的沟通，并着重于尽早发现各种缺陷，Agile将软件开发提升到了一个新的水平。
              </p>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                但是，出现了新的挑战。
              </p>
              
              <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                微服务架构的演变
              </h3>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                现在，我们慢慢开始转向微服务架构，并且开始构建许多小型API，而不是构建大型整体应用程序。
              </p>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                新的挑战是什么？
              </p>
              
              <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                <p style="margin-bottom: 3px;font-size: 12px;">
                  运维变得更加重要。你每周要进行数百个小型微服务发布，而不是一个月发布一个整体。调试微服务中的问题并了解微服务中发生的事情变得很重要。
                </p>
              </blockquote>
              
              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  是时候在软件开发中使用新的流行语了–DevOps。
                </p>
                
                <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  DevOps的出现
                </h3>
                
                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  DevOps的重点是什么？
                </p>
                
                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  DevOps的重点是加强开发与运维团队之间的沟通。
                </p>
                
                <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                  <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                    <li style="margin-bottom: 6px;font-size: 12px;">
                      如何使部署更容易？</p>
                    </li>
                    <li style="margin-bottom: 6px;font-size: 12px;">
                      如何使运维团队更加了解开发团队做的事情？</p>
                    </li>
                  </ul>
                </blockquote>
                
                <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  DevOps如何增强团队之间的沟通？
                </h4>
                
                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  DevOps使运维团队与开发团队更加接近。
                </p>
                
                <ul class="list-paddingleft-2" style='margin-bottom: 18px;margin-left: 50px;width: 577.422px;list-style-position: initial;list-style-image: initial;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  <li style="margin-bottom: 6px;">
                    在更成熟的企业中，开发团队和运维团队是一个团队。他们开始共享共同的目标，并且两个团队都开始了解另一个团队面临的挑战。</p>
                  </li>
                </ul>
                
                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                  <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                    DevOps团队关注的自动化领域是什么？
                  </h4>
                  
                  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                    除了敏捷开发的重点领域（持续集成和测试自动化）外，DevOps团队还致力于帮助使运维团队自动化，例如，在服务器上配置软件，部署应用程序以及监视生产环境。一些关键术语是持续部署和基础架构即代码。
                  </p>
                  
                  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                    持续部署就是要在测试环境上持续部署新版本的软件。在像Google，Facebook这样的更成熟的组织中，Continuous Delivery可帮助将软件连续部署到生产中。
                  </p>
                  
                  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                      基础设施即代码就是将基础设施视为应用程序代码。你可以使用自动化配置方式创建基础结构（服务器，负载平衡器和数据库）。你将对基础结构进行版本控制-以便可以跟踪。
                    </p>
                    
                    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                      <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        DevOps如何促进即时反馈？
                      </h4>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        DevOps将运维和开发团队召集在一起。因为运维和开发都是同一团队的一部分，所以整个团队都了解与运维和开发相关的挑战。
                      </p>
                      
                      <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                        <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                          <li style="margin-bottom: 6px;font-size: 12px;">
                            任何运维问题都会得到开发人员的快速关注。</p>
                          </li>
                          <li style="margin-bottom: 6px;font-size: 12px;">
                            上线软件时遇到的任何挑战，都会引起运维团队的尽早关注。</p>
                          </li>
                        </ul>
                      </blockquote>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        DevOps鼓励将持续集成，持续交付和基础架构作为代码。
                      </p>
                      
                      <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                        <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                          <li style="margin-bottom: 6px;font-size: 12px;">
                            由于持续交付，如果我进行代码更改或配置更改破坏了测试或环境，那么我会在几个小时内知道。</p>
                          </li>
                          <li style="margin-bottom: 6px;font-size: 12px;">
                            由于使用“基础结构即代码”，开发人员可以自行配置环境，部署代码并自行查找问题，而无需运维团队的任何帮助。</p>
                          </li>
                        </ul>
                      </blockquote>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        尽管我们说敏捷开发和DevOps是使事情变得简单的两个不同的事物，但实际上，对于DevOps的含义没有公认的定义。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        我认为敏捷开发和DevOps是两个阶段，可以帮助我们改善构建出色软件。他们不会互相竞争，但是可以一起帮助我们构建出色的软件产品。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        就我而言，敏捷开发和DevOps是相辅相成
                      </p>
                      
                      <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                        <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                          <li style="margin-bottom: 6px;font-size: 12px;">
                            促进业务，开发和运维团队之间的沟通和反馈</p>
                          </li>
                          <li style="margin-bottom: 6px;font-size: 12px;">
                            通过自动化缓解痛点。</p>
                          </li>
                        </ul>
                      </blockquote>
                      
                      <h2 style='margin-top: 18px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-size: 18px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);border-bottom: 1px solid rgb(234, 234, 234);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        DevOps的故事
                      </h2>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        如果你是团队中的明星开发人员，因此软件问题需要你快速解决。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        你要 checkout 项目。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        你要快速创建本地环境。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        你要修改代码。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        你要更新单元测试和自动化测试。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        你要提交提交代码。
                      </p>
                      
                      <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          你会收到一封电子邮件，指出将进行质量检查。
                        </p>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          一些集成测试会自动运行。
                        </p>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          你的质量检查小组会收到一封电子邮件，要求你批准。他们进行手动测试并批准。
                        </p>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          你的代码将在几分钟内投入生产。
                        </p>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          这就是DevOps的故事。
                        </p>
                        
                        <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          DevOps = Development + Operations
                        </h3>
                        
                        <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                          <p style="margin-bottom: 3px;font-size: 12px;">
                            DevOps是软件开发的自然发展。DevOps不仅仅是工具，框架还是自动化。这是所有这些的结合。
                          </p>
                        </blockquote>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          DevOps专注于人员，流程和产品。DevOps的“人员”部分是关于文化和树立良好思维模式的，这是一种促进开放式沟通并重视快速反馈的文化，一种重视高质量软件的文化。
                        </p>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          敏捷开发帮助弥合了业务团队与开发团队之间的鸿沟。开发团队了解业务的优先级，并与业务合作，以提供最有价值的故事为先。但是，开发团队和运维团队并没有保持一致。
                        </p>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          他们有不同的目标。
                        </p>
                        
                        <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                          <p style="margin-bottom: 3px;font-size: 12px;">
                            开发团队的目标是将尽可能多的新功能投入生产。
                          </p>
                        </blockquote>
                        
                        <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                          <p style="margin-bottom: 3px;font-size: 12px;">
                            Ops团队的目标是保持生产环境尽可能稳定。
                          </p>
                        </blockquote>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          如你所见，如果将产品投入生产很困难，那么开发人员和运维人员就无法保持一致。
                        </p>
                        
                        <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                          <p style="margin-bottom: 3px;font-size: 12px;">
                            DevOps旨在使Dev和Ops团队实现共同目标。
                          </p>
                        </blockquote>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          开发团队与运维团队合作，以了解和解决运维难题。Ops团队是Scrum团队的成员，了解开发中的功能。
                        </p>
                        
                        <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                          <p style="margin-bottom: 3px;font-size: 12px;">
                            我们如何才能做到这一点？
                          </p>
                        </blockquote>
                        
                        <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                          <p style="margin-bottom: 3px;font-size: 12px;">
                            打破开发人员和行运维人员之间的隔墙！
                          </p>
                        </blockquote>
                        
                        <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          开发人员和运维人员结合-选项1
                        </h4>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          在成熟的DevOps企业中，Dev和Ops是同一Scrum团队的一部分，彼此分担责任。
                        </p>
                        
                        <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          开发人员和运维人员结合选项2
                        </h4>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          但是，如果你处在DevOps演进的早期阶段，那么如何才能使Dev和Ops拥有共同的目标并一起工作呢？
                        </p>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          你可以执行以下操作：
                        </p>
                        
                        <ul class="list-paddingleft-2" style="width: 577.422px;list-style-type: square;">
                          <li style="margin-bottom: 6px;font-size: 15px;">
                            你可以开始做的一件事就是让开发团队分担运维团队的一些职责。例如，开发团队可以在生产部署后的第一周内负责新版本的发布。这有助于开发团队了解在发布新版本时操作所面临的挑战，并帮助他们团结起来找到更好的解决方案。</p>
                          </li>
                          <li style="margin-bottom: 6px;font-size: 15px;">
                            你可以做的另一件事是让运维团队的代表参与Scrum活动。</p>
                          </li>
                          <li style="margin-bottom: 6px;font-size: 15px;">
                            接下来，你可以使开发团队更清楚地看到运维团队面临的挑战。当你在运维中遇到任何挑战时，请使开发团队成为解决方案团队的一部分。</p>
                          </li>
                        </ul>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          由于自动化，出现了另一个有趣的选择。通过将基础结构即代码并为开发人员启用预配置，你可以创建操作和开发团队可以理解的通用语言-代码。
                        </p>
                        
                        <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          DevOps用例
                        </h3>
                        
                        <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            此图展示了两个简单的工作流程
                          </p>
                          
                          <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                            <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                No 1：使用Terraform和Azure DevOps来配置Kubernetes集群的基础架构。</p>
                              </li>
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                No 2 ：使用Azure DevOps持续部署微服务，以将微服务的Docker镜像构建和部署到Kubernetes群集中。</p>
                              </li>
                            </ul>
                          </blockquote>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            这听起来复杂吗？
                          </p>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            让我们分解一下，尝试并理解它们。
                          </p>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            让我们从No 2开始-首先是持续部署。
                          </p>
                          
                          <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            No 2：使用Azure DevOps和Jenkins进行DevOps连续部署
                          </h4>
                          
                          <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                            <p style="margin-bottom: 3px;font-size: 12px;">
                              如果你不经常运行测试和代码质量检查，这有什么用？
                            </p>
                            
                            <p style="margin-bottom: 3px;font-size: 12px;">
                              如果你没有足够频繁地部署软件，部署自动化有什么用？
                            </p>
                          </blockquote>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            开发人员将代码提交到版本控制系统后，将立即执行以下步骤：
                          </p>
                          
                          <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                            <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                单元测试。</p>
                              </li>
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                代码质量检查。</p>
                              </li>
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                集成测试。</p>
                              </li>
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                应用程序打包—构建应用程序的可部署版本。</p>
                              </li>
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                应用程序部署-启用新版本的应用程序。</p>
                              </li>
                              <li style="margin-bottom: 6px;font-size: 12px;">
                                给测试团队的电子邮件，用于测试应用程序。</p>
                              </li>
                            </ul>
                          </blockquote>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            一旦获得测试团队的批准，该应用程序将立即部署到Next Environment。
                          </p>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            这称为连续部署。如果你持续部署到生产环境，则称为持续交付。
                          </p>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            最受欢迎的CI / CD工具是Azure DevOps和Jenkins
                          </p>
                          
                          <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            No 1：使用Terraform实现DevOps中的基础架构即代码
                          </h4>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            在过去，我们创建环境并手动部署应用程序。
                          </p>
                          
                          <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                              每次创建服务器时，都需要手动完成。
                            </p>
                            
                            <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                              <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                <li style="margin-bottom: 6px;font-size: 12px;">
                                  如果需要更新Java版本怎么办？</p>
                                </li>
                                <li style="margin-bottom: 6px;font-size: 12px;">
                                  是否需要应用安全补丁？</p>
                                </li>
                              </ul>
                            </blockquote>
                            
                            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                              你手动进行。
                            </p>
                            
                            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                              这是什么结果？
                            </p>
                            
                            <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                              <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                <li style="margin-bottom: 6px;font-size: 12px;">
                                  错误几率很高。</p>
                                </li>
                                <li style="margin-bottom: 6px;font-size: 12px;">
                                  复制环境很困难。</p>
                                </li>
                              </ul>
                            </blockquote>
                            
                            <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                              基础架构即代码
                            </h4>
                            
                            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                              基础架构即代码—像应用程序代码一样对待基础架构
                            </p>
                            
                            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                              这是基础架构即代码应理解的一些重要事项
                            </p>
                            
                            <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                              <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                <li style="margin-bottom: 6px;font-size: 12px;">
                                  基础设施团队专注于增值工作（而不是例行工作）。</p>
                                </li>
                                <li style="margin-bottom: 6px;font-size: 12px;">
                                  更少的错误，可以从故障中快速恢复。</p>
                                </li>
                                <li style="margin-bottom: 6px;font-size: 12px;">
                                  服务器是一致的（避免配置漂移）。</p>
                                </li>
                              </ul>
                            </blockquote>
                            
                            <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                最受欢迎的IaC工具是Ansible和Terraform。
                              </p>
                              
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                通常，这些是IaC中的步骤
                              </p>
                              
                              <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                  <li style="margin-bottom: 6px;font-size: 12px;">
                                    通过模板配置服务器（由云启用）</p>
                                  </li>
                                  <li style="margin-bottom: 6px;font-size: 12px;">
                                    安装软件</p>
                                  </li>
                                  <li style="margin-bottom: 6px;font-size: 12px;">
                                    配置软件</p>
                                  </li>
                                </ul>
                              </blockquote>
                              
                              <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                服务器配置
                              </h4>
                              
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                最受欢迎的配置工具是CloudFormation和Terraform。
                              </p>
                              
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                使用Terraform，你可以预配置服务器和基础架构的部分，例如负载平衡器，数据库，网络配置等。你可以使用使用Packer和AMI等工具预创建镜像来创建服务器（Amazon Machine Image）。
                              </p>
                              
                              <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                配置管理
                              </h4>
                              
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                配置管理工具用于
                              </p>
                              
                              <ul class="list-paddingleft-2" style='margin-bottom: 18px;margin-left: 50px;width: 577.422px;list-style-position: initial;list-style-image: initial;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                <li style="margin-bottom: 6px;">
                                  安装软件</p>
                                </li>
                                <li style="margin-bottom: 6px;">
                                  配置软件</p>
                                </li>
                              </ul>
                              
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                流行的配置管理工具是Chef，Puppet，Ansible和SaltStack。这些旨在在现有服务器上安装和管理软件。
                              </p>
                              
                              <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                Docker和Kubernetes在DevOps中的作用
                              </h3>
                              
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                Docker和Kubernetes在DevOps中的作用是什么？
                              </p>
                              
                              <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                  在微服务世界中，可能会使用Java构建一些微服务，使用Python构建一些微服务，以及使用JavaScript构建一些微服务。
                                </p>
                                
                                <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                  <p style="margin-bottom: 3px;font-size: 12px;">
                                    不同的微服务将具有不同的构建和部署方式。
                                  </p>
                                  
                                  <p style="margin-bottom: 3px;font-size: 12px;">
                                    这使运维团队的工作变得困难。
                                  </p>
                                  
                                  <p style="margin-bottom: 3px;font-size: 12px;">
                                    我们如何有相同的方式来部署多种类型的应用程序？容器和Docker。
                                  </p>
                                </blockquote>
                                
                                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                  使用Docker，你可以构建微服务的镜像-不管它们的语言如何。你可以在任何基础架构上以相同方式运行这些镜像。
                                </p>
                                
                                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                  这简化了操作。
                                </p>
                                
                                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                  Kubernetes通过帮助协调不同类型的容器并将其部署到集群。
                                </p>
                                
                                <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                    Kubernetes还提供：
                                  </p>
                                  
                                  <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                    <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                      <li style="margin-bottom: 6px;font-size: 12px;">
                                        服务发现。</p>
                                      </li>
                                      <li style="margin-bottom: 6px;font-size: 12px;">
                                        负载均衡。</p>
                                      </li>
                                      <li style="margin-bottom: 6px;font-size: 12px;">
                                        集中配置。</p>
                                      </li>
                                    </ul>
                                  </blockquote>
                                  
                                  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                    Docker和Kubernetes使DevOps变得容易。
                                  </p>
                                  
                                  <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                    <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      重要的DevOps指标
                                    </h3>
                                    
                                    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      以下是一些重要的DevOps指标。
                                    </p>
                                    
                                    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          部署频率-将应用程序多长时间部署到生产一次？</p>
                                        </li>
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          上线时间-从编码到投入生产，你需要花多长时间？</p>
                                        </li>
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          新版本的失败率-新版本多少次失败？</p>
                                        </li>
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          修复的交付时间-你需要多长时间进行生产修复并将其发布到生产中？</p>
                                        </li>
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          平均恢复时间-从重大问题恢复到部署生产环境需要花费多长时间？</p>
                                        </li>
                                      </ul>
                                    </blockquote>
                                    
                                    <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      DevOps最佳做法
                                    </h3>
                                    
                                    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      以下是DevOps的一些最佳做法
                                    </p>
                                    
                                    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          标准化</p>
                                        </li>
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            具有跨职能技能的团队
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            关注团队文化
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            自动化，自动化，自动化。
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            不变的基础设施
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            开发和生产环境相同
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            版本控制
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            自我配置管理
                                          </p>
                                        </li>
                                      </ul>
                                    </blockquote>
                                    
                                    <h3 style='margin-top: 20px;margin-bottom: 18px;padding-top: 10px;padding-bottom: 10px;font-weight: bold;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      如何衡量DevOps实施的成熟度
                                    </h3>
                                    
                                    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      你如何衡量DevOps实施的成熟度？这里是一些重要的问题。
                                    </p>
                                    
                                    <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      <span style="font-weight: 700;">开发</span>
                                    </h4>
                                    
                                    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          每次提交都会触发自动测试和自动代码质量检查吗？</p>
                                        </li>
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你的代码是否持续交付生产？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你使用结对编程( pair programming )吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你使用TDD和BDD吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你有很多可重复使用的模块吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            开发团队可以自我配置环境吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            快速修复生产需要多长时间？
                                          </p>
                                        </li>
                                      </ul>
                                    </blockquote>
                                    
                                    <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      <span style="font-weight: 700;">测试</span>
                                    </h4>
                                    
                                    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你的测试是否自动化？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            自动化测试失败时，构建会失败吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你的测试周期短吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你有自动NFR测试吗？
                                          </p>
                                        </li>
                                      </ul>
                                    </blockquote>
                                    
                                    <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      <span style="font-weight: 700;">部署方式</span>
                                    </h4>
                                    
                                    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你是否具有开发和生产环境相同？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你是否使用A / B测试？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你是否使用金丝雀部署？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            只需单击一下按钮即可部署？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你可以通过单击按钮来回滚吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你能否通过单击按钮设置和释放基础结构？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            你是否将IAC和版本控制用于基础架构？
                                          </p>
                                        </li>
                                      </ul>
                                    </blockquote>
                                    
                                    <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      <span style="font-weight: 700;">监控方式</span>
                                    </h4>
                                    
                                    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            团队是否使用集中监控系统？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            只需单击一个按钮，开发团队就可以访问日志吗？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            如果生产中出现问题，团队是否会收到警报？
                                          </p>
                                        </li>
                                      </ul>
                                    </blockquote>
                                    
                                    <h4 style='margin-top: 10px;margin-bottom: 10px;font-size: 15px;font-family: "Microsoft Yahei";line-height: 1.1;color: rgb(85, 85, 85);text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      <span style="font-weight: 700;">团队与流程</span>
                                    </h4>
                                    
                                    <blockquote style='margin: 30px;padding: 15px 14px 10px;border-left-width: 5px;border-left-color: rgba(51, 102, 204, 0.67);color: rgb(136, 136, 136);font-size: 16px;background-color: rgb(238, 238, 238);font-family: "Microsoft Yahei";text-align: start;white-space: normal;'>
                                      <ul class="list-paddingleft-2" style="margin-left: 50px;width: 484.5px;list-style-position: initial;list-style-image: initial;">
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            团队是否希望不断改进？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            团队是否具备业务，开发和运维所需的全部技能？
                                          </p>
                                        </li>
                                        
                                        <li style="margin-bottom: 6px;font-size: 12px;">
                                          <p>
                                            团队是否跟踪关键的DevOps指标并对其进行改进？
                                          </p>
                                        </li>
                                      </ul>
                                    </blockquote>
                                    
                                    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      译者：王延飞
                                    </p>
                                    
                                    <p style='margin-bottom: 18px;color: rgb(85, 85, 85);font-family: "Microsoft Yahei";font-size: 15px;text-align: start;white-space: normal;background-color: rgb(255, 255, 255);'>
                                      原文链接：https://dzone.com/articles/devops-tutorial-devops-with-docker-kubernetes-and#
                                    </p>
                                    
                                    <p style="white-space: normal;">
                                    </p><section style="white-space: normal;max-width: 100%;letter-spacing: 0.544px;line-height: 1.75em;text-align: center;box-sizing: border-box !important;overflow-wrap: break-word !important;">
                                    
                                    <span style="max-width: 100%;font-size: 15px;color: rgb(85, 85, 85);letter-spacing: 0.544px;widows: 1;caret-color: rgb(51, 51, 51);box-sizing: border-box !important;overflow-wrap: break-word !important;">END</span></section> 
                                    
                                    <hr style="margin-top: 28px;white-space: normal;max-width: 100%;letter-spacing: 0.544px;text-align: center;widows: 1;caret-color: rgb(63, 63, 63);color: rgb(63, 63, 63);font-family: Avenir, -apple-system-font, 微软雅黑, sans-serif;font-size: 16px;text-size-adjust: auto;height: 1px;border-top-style: dotted;border-top-color: rgb(165, 165, 165);box-sizing: border-box !important;overflow-wrap: break-word !important;" />
                                    <section style="margin-top: 15px;margin-bottom: 10px;white-space: normal;max-width: 100%;min-height: 1em;letter-spacing: 0.544px;text-align: center;box-sizing: border-box !important;overflow-wrap: break-word !important;">
                                    <a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI5ODQ2MzI3NQ==&mid=2247489047&idx=2&sn=b0fe10a93eb8fc1f1edbb47dc92699ff&chksm=eca42f53dbd3a6458f556ee8561fc293ca02bc328492d6b056df60b3116060268a96b959563c&scene=21#wechat_redirect" textvalue="Kubernetes  CKA线下班" data-itemshowtype="0" tab="innerlink" data-linktype="2" rel="noopener noreferrer"><span style="max-width: 100%;-webkit-tap-highlight-color: rgba(0, 0, 0, 0);cursor: pointer;font-size: 15px;box-sizing: border-box !important;overflow-wrap: break-word !important;">Kubernetes  CKA线下班</span></a></section> <section style="margin-bottom: 10px;white-space: normal;max-width: 100%;min-height: 1em;letter-spacing: 0.544px;text-align: center;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a target="_blank" href="http://mp.weixin.qq.com/s?__biz=MzI5ODQ2MzI3NQ==&mid=2247489047&idx=2&sn=b0fe10a93eb8fc1f1edbb47dc92699ff&chksm=eca42f53dbd3a6458f556ee8561fc293ca02bc328492d6b056df60b3116060268a96b959563c&scene=21#wechat_redirect" textvalue="你已选中了添加链接的内容" data-itemshowtype="0" tab="innerlink" data-linktype="1" rel="noopener noreferrer"><span class="js_jump_icon h5_image_link" data-positionback="static" style="top: auto;left: auto;margin: 0px;right: auto;bottom: auto;"></span></a></section> 
                                    
                                    <p style="white-space: normal;">
                                    </p>
                                    
                                    <p style="font-size: 16px;font-variant-numeric: normal;font-variant-east-asian: normal;white-space: normal;max-width: 100%;min-height: 1em;color: rgb(51, 51, 51);font-family: 微软雅黑;letter-spacing: 0.544px;widows: 1;text-align: center;line-height: 1.75em;background-color: rgb(255, 255, 255);box-sizing: border-box !important;overflow-wrap: break-word !important;">
                                      <section data-role="outer" label="Powered by 135editor.com" style="font-size: 16px;font-variant-numeric: normal;font-variant-east-asian: normal;white-space: normal;max-width: 100%;color: rgb(51, 51, 51);font-family: 微软雅黑;letter-spacing: 0.544px;line-height: 25.6px;text-align: justify;widows: 1;background-color: rgb(255, 255, 255);box-sizing: border-box !important;overflow-wrap: break-word !important;"> <section data-role="outer" label="Powered by 135editor.com"> 
                                      
                                      <p style="text-align: center;">
                                        </section> </section>