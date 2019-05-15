---
title: JS获取URL中参数值
url: 105.html
id: 105
categories:
  - js
date: 2018-02-23 13:50:00
tags:
---

**function** GetQueryString(name) { **var** reg = **new** RegExp("(^|&)" + name + "=(\[^&\]*)(&|$)"); **var** r = window.location.search.substr(1).match(reg); **if** (r != **null**)**return** decodeURI(r\[2\]); **return null**; }