---
title: 微信浏览器input关闭键盘后导致页面底部空缺问题解决方案
url: 202.html
id: 202
categories:
  - 未分类
date: 2019-03-14 18:53:57
tags:
---

这个问题是在部分ios手机里出现的，目前安卓手机没有复现。

猜测：在微信webview打开我们h5页面的时候，就固定了页面的高度，如果这个input在页面的底部，当呼出软键盘时，由于高度问题，整个webview会被键盘顶上去，而取消时没有恢复原状。

解决办法：

绑定一个blur事件，当其触发时，使scrollTo为0

付代码如下：

          <input
            type="text"
            class="inputValue"
            v-model="teamCodeValue"
            @blur="inputBlur"
            placeholder="说明"
          >
    //对应的methods中添加js
    inputBlur () {
      window.scrollTo(0, 0)
    },

方可解决  亲测有效
----------

作者：Song-宋-Song  
来源：CSDN  
原文：https://blog.csdn.net/qq_40028324/article/details/85333956  
版权声明：本文为博主原创文章，转载请附上博文链接！