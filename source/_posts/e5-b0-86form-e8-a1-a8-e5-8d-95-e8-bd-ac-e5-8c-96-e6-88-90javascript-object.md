---
title: 将form表单转化成Javascript object
url: 107.html
id: 107
categories:
  - js
date: 2018-02-23 13:51:29
tags:
---

$.fn.serializeObject = function() { var o = {}; var a = this.serializeArray(); $.each(a, function() { if (o\[this.name\]) { if (!o\[this.name\].push) { o\[this.name\] = \[ o\[this.name\] \]; } o\[this.name\].push(this.value || ''); } else { o\[this.name\] = this.value || ''; } }); return o; }