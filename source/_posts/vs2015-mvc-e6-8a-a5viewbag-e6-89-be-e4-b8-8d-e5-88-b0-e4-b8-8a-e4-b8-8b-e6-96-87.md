---
title: VS2015 MVC 报ViewBag找不到上下文
url: 83.html
id: 83
categories:
  - 配置
date: 2018-02-23 11:43:06
tags:
---

打开文件夹 Users\\<CurrentUser>\\AppData\\Local\\Microsoft\\VisualStudio\\<version> 删除文件夹 ComponentModelCache 重启 Visual Studio. 参考链接：[http://stackoverflow.com/questions/25573424/vs2013-error-loading-solution-javascriptwebextensionspackage-did-not-load-cor/25573496](http://stackoverflow.com/questions/25573424/vs2013-error-loading-solution-javascriptwebextensionspackage-did-not-load-cor/25573496)