---
title: jquery ajax请求完整写法
url: 71.html
id: 71
categories:
  - js
date: 2018-02-23 11:34:32
tags:
---

var ajaxTimeoutTest = $.ajax({ url: 'url',  //请求的URL timeout: 10000, //超时时间设置，单位毫秒 type: 'get',  //请求方式，get或post data: {},  //请求所传参数，json格式 dataType: '',//返回的数据格式 success: function (result) { //请求成功的回调函数 if (result) {   } }, error: function () {   }, complete: function (XMLHttpRequest, status) { //请求完成后最终执行参数 if (status == 'timeout') {//超时,status还有success,error等值的情况 ajaxTimeoutTest.abort(); } } });