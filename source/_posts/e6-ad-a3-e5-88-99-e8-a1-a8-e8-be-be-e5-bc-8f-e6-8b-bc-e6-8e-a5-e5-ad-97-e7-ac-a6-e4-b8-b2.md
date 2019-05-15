---
title: 正则表达式拼接字符串
url: 73.html
id: 73
categories:
  - js
date: 2018-02-23 11:35:20
tags:
---

var a  = ‘1,2,3’ var reNum = new RegExp('^\[0-9\]+(\[.\]\\\d{' + a + '})?$');