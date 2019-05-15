---
title: 关于在移动端 body overflow：hidden 无效的问题
url: 205.html
id: 205
categories:
  - 未分类
date: 2019-03-22 16:55:10
tags:
---

关于在移动端 body overflow：hidden 无效的问题

![](//upload-images.jianshu.io/upload_images/2243106-2126781aba9c7466.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

一、简述

这篇文章作为一道前菜哈。

很多同学在做移动端的时候都有遇到移动端各种坑，各种固定fiexd失效，input输入框错位，软键盘弹出错位等等….但是，在做一张单页面的页面时，尤其是以整个页面去加载背景图片的时候，背景图片固定就很有必要了。

这里先讨论一下body关于overflow的问题，移动端固定的问题会在下一篇文章讨论。

二、简单演示

很多时候都是想着使用body去固定:

    body{

                position:fixed; //这个属性暂时不提，重点看overflow

                overflow:hidden;

    }

1、这样的效果在「PC端」是可以的，整个页面时被固定的：

em….感觉说的好空，有句话说得好**「talk is easy ,show me the code」** ，上个demo ：

![](//upload-images.jianshu.io/upload_images/2243106-27bd3df19584f413.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

em…上完code，再来「PC端」的效果演示：

![](//upload-images.jianshu.io/upload_images/2243106-7265ffb87afa2444.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

OK，上面GIF演示的效果已经告诉我们，body的**overflow：hidden**在「PC端」是有效果的。

em..接下来是重点好吧，说说在移动端的body 如何不行？

2、同样的样式在「移动端」是可以滑动的，可以滑动就是不合理了。

如何看手机的上样式效果对不对，我这里稍微讲2种方式哈：

（**A.**说用「微信开发者工具」的同学，我这里就不讲了，这也是一种方法可以查看移动端的样式效果，尤其是要用在微信网页中的，是一个不错的推荐。**B.**说用模拟器的同学，我也不想说太多了，查看一个简单的样式，难道要全开安卓和iOS 模拟器？）

2.1、放到服务器上

这样，我把网页放到了我个人的服务器上，比较简单粗暴的做法，很通用哈，一输网址就可以看到了，下面是把网页放到手机之后的效果：

![](//upload-images.jianshu.io/upload_images/2243106-ebc6eb27e2517d2a.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/544/format/webp)

看到上面效果，what！！！body的hidden:overflow确实在「移动端」真是无效的，很明显的是可以滑动..

这时IOS手机上的效果，在安卓手机上同样也是一样的效果，这里我就不录屏了，有点累呀

细心的同学可以发现我手机上的网页会出现蓝色，橙色的遮罩，em...get 到我这个点了，上面演示的是第二种方法里的一些调试。

2.2使用远程web调试工具-----weinre

️**注意！注意！注意！使用的是weinre这款远程调试工具，重要的事情讲一遍。**

![](//upload-images.jianshu.io/upload_images/2243106-a9b1330548560d96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/622/format/webp)

上面说的就是可以使用这个款远程web页面调试工具，一款稍显老的工具，不过**好在可以跨平台全终端使用，可以无线查看和调试dom和css样式，**但是不可以修改dom,不可以断点调试…em…缺点比较多，但是用来查看这个demo是妥妥的。

      有兴趣的同学可以找一下weinre的使用方法。（或者空闲的时候写一下使用weinre的基本教程和心得）

原理就是需要在原网页中插入一段js代码，实现中间的target，用于传输调试请求，请保持在同一网络上。

️ **重点来了！重点来了！重点来了！**

作用： **2.2.1、可以实现移动端可以无线直接查看本地电脑编写的网页，而无需放置到服务器上。**

                    **2.2.2、可以实现在电脑直接对移动端进行dom和样式调试，无线连接调试。**

可以上个图看一下效果；

第一种情况：就是我直接在weinre目录中添加需要查看的文件，在手机上可以直接访问电脑ip，获取本地编写的网页。

上个图先：

**～文件所在：**

![](//upload-images.jianshu.io/upload_images/2243106-6b5a01a45452d2ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/414/format/webp)

**～手机访问：**

![](//upload-images.jianshu.io/upload_images/2243106-99eb6f4486319678.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

        可以清晰的看到手机和电脑处于同一局域网，手机正在访问到电脑本地的网页。

            第二种情况：我把网页放到了我的服务器上，手机访问可以直接查看，在电脑上也是可以对手机的样式进行调试的，如下图：

![](//upload-images.jianshu.io/upload_images/2243106-30ec766d9a0500ef.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

          这里是可以看到我通过网络访问进行的，并在浏览器出现类似于chrome dev调试工具的页面，正在对手机样式进行调试。

            **好吧综上所述，可以看到weinre是可以又这个能力对移动端进行样式调试的，并且证明说overflow:hidden在移动端确实是可以进行无障碍的滑动。**

          ️️️ 回到主题！回到主题！回到主题！

三、结论以及解决方法

      事实证明了hidden在移动端无效。原因总结来说是移动端的坑，对网页兼容的问题；我觉得是移动端网页的html页面本身也是可以「上下拉扯滑动1」的，而body的position位置默认是static，不做修改的情况下是直接随着html页面「滑动1」，而body可以直接在html内实现「滑动2」，（两个滑动是不一样的滑动，「滑动1」指的是整个html相对于浏览器窗口滑动，「滑动2」指的是整个body相对于html页面滑动）

下面是「滑动1」的如图：

![](//upload-images.jianshu.io/upload_images/2243106-6ab5a94a0a1fdd29.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

「滑动2」就是我们本身可以看到的那种效果。

    解决方法：

    通过父节点固定，像body的父节点是html，可以通过固定父节点html， 来把其下的字节点固定，当然字节点前提是不做脱离文档流的行为。

**    html{**

**          position:fixed;**

**            height:100%;**

**            width:100%;**

**    }**

  完整代码如下：

![](//upload-images.jianshu.io/upload_images/2243106-f0f424c38594fc6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/959/format/webp)

这里剧个透，有个细节的地方是：

比较多的同学会直接不添加父节点固定，而是直接把body给赋值position:fixed了；如下

![](//upload-images.jianshu.io/upload_images/2243106-72f7e63be828f2d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/478/format/webp)

**直接这样写的，很明显在手机端可以看到，body是固定的，但是整个html页面在滑动，这就达不到固定的预期的要求了。同时这样子很容易会造成一种错觉，在接下来写的元素中会让人觉得是整个element在自己滑动，其实不是，是html页面在滑动。**

嗯嗯，总结来说就是这样子。有关于移动端的坑可以交流一下，我这里刚好有些链接，本人觉得是挺不错的各种移动端的坑的总结。

作者：Cc卿  
链接：https://www.jianshu.com/p/526ed6e6b92e  
来源：简书