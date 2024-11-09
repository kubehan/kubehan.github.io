---
title: 如何把Execl表和拆分成单个的excel表格文件
author: Kubehan
type: post
date: 2020-09-24T05:36:56+08:00
url: /2902.html
featured_image: https://www.kubehan.cn/wp-content/uploads/2020/09/1600925766-dc3c0858226a532.png
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 1369
categories:
  - 小技巧

---
# 如何把Execl表和拆分成单个的excel表格文件

需求：

现在我有一个excel文件，里面有1000张表，领导要求把这些表中的某100个表单独发给他！  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1600925522-d17ea044a9eb989.png" alt="file" /> 

疑问：难道我要把这100张表一张一张的复制出来然后再发给老板吗？no no no

绝招：拆分表

开始教程

**步骤1：**打开您的Excel工作簿，然后单击**开发工具**选项卡下的“**Visual Basic”**命令，或者只需按“**ALT + F11**”快捷方式。如果没有开发工具一栏，直接按快捷键  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1600925570-e61968137bcea77.png" alt="file" /> 

**步骤2：**出现“**Visual Basic编辑器**”窗口 单击“**插入**” – >“**模块**”以创建新模块。  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1600925766-dc3c0858226a532.png" alt="file" /> 

**步骤3：**将以下VBA代码粘贴到代码窗口中。然后单击“**保存**”按钮

<pre><code class="language-vbscript">Sub SplitWorkbook()
    Dim workbookPath As String
    workbookPath = Application.ActiveWorkbook.Path
    Application.ScreenUpdating = False
    Application.DisplayAlerts = False
    For Each wSheet In ThisWorkbook.Sheets
        wSheet.Copy
        Application.ActiveWorkbook.SaveAs Filename:=workbookPath & "\" & wSheet.Name & ".xlsx"
        Application.ActiveWorkbook.Close False
    Next
    Application.DisplayAlerts = True
    Application.ScreenUpdating = True
End Sub</code></pre>

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1600925619-0e529c6d8874cf1.png" alt="file" /> 

步骤4：然后运行上面的excel宏。单击**运行**命令  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1600925721-803c6cfd72a985d.png" alt="file" />  
<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1600925663-baeee53f3b6d0fe.png" alt="file" /> 

## 见证奇迹

<img decoding="async" src="https://www.kubehan.cn/wp-content/uploads/2020/09/1600925675-f57697a74c63958.png" alt="file" /> 

所有的表格全部变成单个文件