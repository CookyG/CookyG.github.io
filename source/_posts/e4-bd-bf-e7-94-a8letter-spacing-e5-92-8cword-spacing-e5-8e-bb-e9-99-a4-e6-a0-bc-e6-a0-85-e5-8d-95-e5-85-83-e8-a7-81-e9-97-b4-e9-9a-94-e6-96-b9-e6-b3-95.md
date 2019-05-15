---
title: 使用letter-spacing和word-spacing去除格栅单元见间隔方法
url: 93.html
id: 93
categories:
  - css
date: 2018-02-23 13:41:51
tags:
---

下面展示的是[YUI 3 CSS Grids ](http://yuilibrary.com/yui/docs/cssgrids/)使用letter-spacing和word-spacing去除格栅单元见间隔方法（注意，其针对的是block水平的元素，因此对IE8-浏览器做了hack处理）： .yui3-g {    letter-spacing: -0.31em; /* webkit */    \*letter-spacing: normal; /\* IE < 8 重置 */    word-spacing: -0.43em; /* IE < 8 && gecko */} .yui3-u {    display: inline-block;    zoom: 1; \*display: inline; /\* IE < 8: 伪造 inline-block */    letter-spacing: normal;    word-spacing: normal;    vertical-align: top;} 以下是一个名叫[RayM](http://raym31.home.comcast.net/)的人提供的方法： li {    display:inline-block;    background: orange;    padding:10px;    word-spacing:0;    }ul {    width:100%;    display:table;  /* 调教webkit*/    word-spacing:-1em;} .nav li { *display:inline;}