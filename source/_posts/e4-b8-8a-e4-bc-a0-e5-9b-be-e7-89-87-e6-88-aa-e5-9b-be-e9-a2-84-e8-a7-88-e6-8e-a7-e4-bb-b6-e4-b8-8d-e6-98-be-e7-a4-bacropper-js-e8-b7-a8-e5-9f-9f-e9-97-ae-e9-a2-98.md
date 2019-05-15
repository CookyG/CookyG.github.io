---
title: 上传图片截图预览控件不显示cropper.js 跨域问题
url: 91.html
id: 91
categories:
  - html
  - js
date: 2018-02-23 13:38:27
tags:
---

对于高版本（我用的Cropper v2.3.4）因为代码调整了，找到 function getCrossOrigin(crossOrigin)，将里面返回跨域代码的那行注释掉，返回空字符串就好了。 我是这样改的： function getCrossOrigin(crossOrigin) { //return crossOrigin ? ' crossOrigin="' + crossOrigin + '"' : ''; return ''; } 可以显示出来了