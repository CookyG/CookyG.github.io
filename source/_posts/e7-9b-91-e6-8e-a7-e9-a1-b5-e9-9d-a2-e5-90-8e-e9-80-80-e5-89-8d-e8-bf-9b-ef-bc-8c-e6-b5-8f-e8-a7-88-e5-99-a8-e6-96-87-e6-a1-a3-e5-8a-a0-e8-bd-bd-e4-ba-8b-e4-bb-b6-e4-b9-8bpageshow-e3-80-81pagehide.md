---
title: 监控页面后退前进，浏览器文档加载事件之pageshow、pagehide
url: 190.html
id: 190
categories:
  - 未分类
date: 2019-02-02 11:15:29
tags:
---

#### 原文:https://www.cnblogs.com/milo-wjh/p/6811868.html

**发现了问题，首先分析一下呗：**
------------------

*浏览器前进、后退使用机制
-------------

*如何在前进、后退后执行js
--------------

### 一. 问题一：浏览器前进、后退使用机制

用户点击浏览器工具栏中的后退按钮，或者移动设备上的返回键时，或者JS执行history.go(-1);时，浏览器会在当前窗口“打开”历史纪录中的前一个页面。不同的浏览器在“打开”前一个页面的表现上并不统一，这和浏览器的实现以及页面本身的设置都有关系。在浏览器中，“后退到前一个页面”意味着：前一个页面的html/js/css等静态资源的请求（甚至是ajax动态接口请求）根本不会重新发送，直接使用缓存的响应，而不管这些静态资源响应的缓存策略是否被设置了禁用状态。  
各个浏览器测试demo：先点击按钮使其变色，然后调整至百度，再后退回来查看:

    function dispLog(msg) {
      var d = new Date();
      $("<li />").text(d.toISOString().substr(14, 9) + " " + msg)
      .appendTo("#dvDisp");
    }
    
    window.addEventListener('load', function(event) {
        dispLog("Load Event  ");
    })
    //引入了jq监听ready事件做比较
    $(window).ready(function () {
        dispLog("Ready Event");
     });
    $("#btnSetColor").click(function () {
        $("#btnSetColor").css("background", "red");
    })
    window.addEventListener('pageshow', function(event) {
        dispLog("PageShow Event  " + event.persisted);
        //使用event.persisted判断页面是否使用bfcache，值为true或false;  注：如果使用jq绑定事件，需使用event.originalEvent.persisted;
    })
    window.addEventListener('pagehide', function(event) {
        dispLog("Load Event  " + event.persisted);
    })
                

![](https://images2015.cnblogs.com/blog/1091419/201705/1091419-20170508150731941-1926847617.png)

线上测试地址： [pageshow](http://www.milo-wjh.top/blog/pageshow/pageshow_test.html)  
移动端测试demo:  

#### 本人的测试结果：

#### 1.PC端浏览器

浏览器

首次进入页面

点击按钮，再跳转至百度，再后退

chrome 58.0.3029.9

执行ready→load→pageshow

重新执行ready→load→pageshow 按钮颜色消失 页面相当于重新刷新了

IE789

执行ready→load

重新执行ready→load 按钮颜色消失 页面相当于重新刷新了 pageshow不兼容IE10-

firefox 53.0.2

执行ready→load→pageshow

重新执行ready→load→pageshow 按钮颜色消失 页面相当于重新刷新了

QQBrowser 9.6

执行ready→load→pageshow

重新执行ready→load→pageshow 按钮颜色消失 页面相当于重新刷新了

360Browser 8.1.1.254

执行ready→load→pageshow

重新执行ready→load→pageshow 按钮颜色消失 页面相当于重新刷新了

Safari 5.1.7（没mac，用的很早版本的windowsafari）

执行ready→load→pageshow

新增→pagehide→pageshow 按钮颜色红色 引用了缓存（event.persisted == true）

![safari](https://images2015.cnblogs.com/blog/1091419/201705/1091419-20170508161309816-507695064.png)

#### PC端总结：现代的PC浏览器后退后基本都是重新执行load，对页面重新进行一次加载，所以不会有上面的验证码不刷新的问题，由于穷屌没有mac电脑，只下载了很久之前的safari浏览器，测试结果只做参考；

#### 2.移动端浏览器 （安卓端）,测试机型小米5S,iphone4s

浏览器

首次进入页面

点击按钮，再跳转至百度，再后退

UCBrowser

执行ready→load→pageshow

新增→pagehide→pageshow 按钮颜色红色 引用了缓存（event.persisted == true）

QQBrowser

执行ready→load→pageshow

新增→pagehide→pageshow 按钮颜色红色 引用了缓存（event.persisted == true）

QQ内置浏览器

执行ready→load→pageshow

重新执行ready→load→pageshow 按钮颜色消失 页面相当于重新刷新了

360Browser

执行ready→load→pageshow

无触发事件，按钮颜色红色 引用了缓存（event.persisted == true）

chorme

执行ready→load→pageshow

重新执行ready→load→pageshow 按钮颜色消失 页面相当于重新刷新了

微信内置

执行ready→load→pageshow

重新执行ready→load→pageshow 按钮颜色消失 页面相当于重新刷新了

小米自带

执行ready→load→pageshow

无触发事件，按钮颜色红色 引用了缓存（event.persisted == true）

safari

执行ready→load→pageshow

新增→pagehide→pageshow 按钮颜色红色 引用了缓存（event.persisted == true）

效果图：选了三种类型各一个

刷新页面类型

不刷新不执行类型

不刷新执行事件类型（理想型）

    这就很头疼了，三种不同的情况，对于chrome和UC类型的情况，对于我的回退验证码刷新问题，很容易处理，最麻烦的360类型的，但是。。。发现了更蛋疼的情况了：
    后退不reload，不执行pageshow的浏览器在公司的一个项目中，居然能实现后退刷新。。。。 同一段测试代码，发到项目服务器上，是可以后退刷新，
    但是，在我自己的虚拟主机中却不行，南蝶？是因为请求头部设置吗？？？ 先挖个坑在这，等我去好好了解一下PHP 和 http请求 再回来战！

* * *

### 二、如何在前进、后退后执行js

百度、google类似问题，最多的解决方法：pageshow;

> Firefox和Opera有一个新特性，名叫“往返缓存”（back-forward cache，或bfcache），可以在用户使用浏览器的“后退”和“前进”按钮时加快页面的转换速度。  
> 这个缓存中不仅保存着页面数据，还保存了DOM和JavaScript的状态；实际上是将整个页面都保存在了内存里。  
> 如果页面位于bfcache中，那么再次打开该页面就不会触发load事件。  
> 尽管由于内存中保存了整个页面的状态，不触发load事件也不应该会导致什么问题，但为了更形象地说明bfcache的行为，Firefox还是提供了一些新事件。  
> 第一个事件就是pageshow，这个事件在页面显示时触发，无论页面是否来自bfcache。在重新加载页面中，pageshow会在load事件触发后触发；  
> 而对于bfcache中的页面，pageshow会在页面状态完全恢复的那一刻触发。另外要注意的是，虽然这个事件的目标是document，但必须将其事件处理程序添加到window。