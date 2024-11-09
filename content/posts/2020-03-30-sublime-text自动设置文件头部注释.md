---
title: Sublime Text自动设置文件头部注释
author: Kubehan
type: post
date: 2020-03-30T02:13:34+08:00
url: /2055.html
views:
  - 961
  - 961
classic-editor-remember:
  - classic-editor
  - classic-editor
thumbnail:
  - https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText4.png
  - https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText4.png
zm_like:
  - 2
  - 2
categories:
  - Linux运维

---
## 使用的插件：

FileHeader

这个插件是我使用过的感觉比较好用，有简单的插件了

主要功能有：自动添加文件头部注释和给没有注释的文件添加头部注释

上面两个功能是我主要用的，换几个电脑，都在用Sublime Text，好几次都忘记了怎么设置文件头部注释，这回彻底记录一下

### 如何安装插件？

  1. <div style="background: #fefef2;">
      <span style="color: black; font-family: Verdana; font-size: 10pt;">ctrl + shift + p<br /> </span>
    </div>

  2. <div style="background: #fefef2;">
      <span style="color: black; font-family: Verdana; font-size: 10pt;">install package<br /> </span>
    </div>

  3. <div style="background: #fefef2;">
      <span style="color: black; font-family: Verdana; font-size: 10pt;">FileHeader<br /> </span>
    </div>
    
    <p style="background: #fefef2;">
      <span style="color: black; font-size: 10pt;"><span style="font-family: 宋体;">安装成功后该怎么配置文件头部信息？</span><span style="font-family: Verdana;"><br /> </span></span>
    </p>

  4. <div style="background: #fefef2;">
      <span style="color: black; font-size: 10pt;"><span style="font-family: 宋体;">打开</span><span style="font-family: Verdana;"> Preference -> Package Setting -> FileHeader->Setting Default<br /> </span></span>
    </div>

  5. <div style="background: #fefef2;">
      <span style="color: black; font-size: 10pt;"><span style="font-family: 宋体;">打开</span><span style="font-family: Verdana;"> Preference -> Package Setting -> FileHeader-> Setting User<br /> </span></span>
    </div>

  6. <div style="background: #fefef2;">
      <span style="color: black; font-size: 10pt;"><span style="font-family: 宋体;">一般我们把自己的配置放在</span><span style="font-family: Verdana;"> user </span><span style="font-family: 宋体;">中：</span><span style="font-family: Verdana;"><br /> </span></span>
    </div>

  7. <div style="background: #fefef2;">
      <span style="color: black; font-size: 10pt;"><span style="font-family: 宋体;">设置自己的默认参数。</span><span style="font-family: Verdana;"><br /> </span></span>
    </div>
    
    <p style="background: #fefef2; margin-left: 21pt;">
      <span style="color: black; font-size: 10pt;"><span style="font-family: 宋体;">我的默认参数如下：</span><span style="font-family: Verdana;"><br /> </span></span>
    </p>
    
    <p style="background: #fefef2; margin-left: 21pt;">
      <img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText1.png" alt="" /><span style="color: black; font-family: Verdana; font-size: 10pt;"><br /> </span>
    </p>
    
      1. <div style="background: #fefef2;">
          <span style="color: black; font-size: 10pt;"><span style="font-family: 宋体;">上面的是公共的参数</span><span style="font-family: Verdana;"><br /> </span></span>
        </div>
    
    ### 如何根据文件类型使用不同的头部注释呢？
    
    <span style="color: black; font-size: 10pt;"><span style="font-family: 等线; background-color: #fefef2;">那支持的文件在哪里呢，怎么配置不同类型的文件放置位置。打开插件文件夹，在</span><span style="font-family: Verdana; background-color: #fefef2;"> template </span><span style="font-family: 等线; background-color: #fefef2;">中会发现有</span><span style="font-family: Verdana; background-color: #fefef2;"> header </span><span style="font-family: 等线; background-color: #fefef2;">和</span><span style="font-family: Verdana; background-color: #fefef2;"> body </span><span style="font-family: 等线; background-color: #fefef2;">两个文件夹。这里就是配置文件中默认的配置参数位置。</span><span style="font-family: Verdana; background-color: #fefef2;"><br /> </span></span>
    
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText2.png" alt="" /> <span style="color: black; font-family: Verdana; font-size: 10pt; background-color: #fefef2;"><br /> </span>
    
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText3.png" alt="" /> <span style="color: black; font-family: Verdana; font-size: 10pt; background-color: #fefef2;"><br /> </span>
    
    <span style="color: black; font-size: 10pt;"><span style="font-family: 等线; background-color: #fefef2;">注意上面的文件路径，</span><span style="font-family: Verdana; background-color: #fefef2;"><br /> </span></span>
    
    <span style="color: black; font-size: 10pt;"><span style="font-family: 等线; background-color: #fefef2;">修改我们想要添加的文件类型名的内容，</span><span style="font-family: Verdana; background-color: #fefef2;"><br /> </span><span style="font-family: 等线; background-color: #fefef2;">增加我们的参数即可。其中有系统默认值，和我们上面设置的值，作为变量放在</span><span style="font-family: Verdana; background-color: #fefef2;"> {{}} </span><span style="font-family: 等线; background-color: #fefef2;">中，例如：修改</span><span style="font-family: Verdana; background-color: #fefef2;">shell</span><span style="font-family: 等线; background-color: #fefef2;">文件的头部注释</span><span style="font-family: Verdana; background-color: #fefef2;"><br /> </span></span>
    
    <span style="color: black; font-size: 10pt;"><span style="font-family: 等线; background-color: #fefef2;">打开下图文件写入内容</span><span style="font-family: Verdana; background-color: #fefef2;"><br /> </span></span>
    
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText4.png" alt="" /> <span style="color: black; font-family: Verdana; font-size: 10pt; background-color: #fefef2;"><br /> </span>
    
    #!/bin/bash
    
    \# @Author: {{author}}
    
    \# @Date: {{create_time}}
    
    \# @Last Modified by: {{last\_modified\_by}}
    
    \# @Last Modified time: {{last\_modified\_time}}
    
    \# @E-mail: {{email}}
    
    重启后新建文件就设置了文件头注释了
    
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText5.png" alt="" /> 
    
    上面是添加和生成的操作方式
    
    自动生成的文件头部信息
    
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/03/033020_0213_SublimeText6.png" alt="" /> </li> </ol>