---
title: 在vue中监听window事件
url: 188.html
id: 188
categories:
  - 未分类
date: 2019-02-02 11:07:33
tags:
---

    methods: {
      handleWinFocus() {
        alert('you are in page now');
      },
    },
    ready() {
      window.addEventListener('focus', this.handleWinFocus);
    },
    beforeDestroy() {
      window.removeEventListener('focus', this.handleWinFocus);
    },