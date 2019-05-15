---
title: ES5 部署Object.is
url: 148.html
id: 148
categories:
  - js
date: 2018-04-10 15:41:06
tags:
---

    Object.defineProperty(Object, 'is', {
      value: function(x, y) {
        if (x === y) {
          // 针对+0 不等于 -0的情况
          return x !== 0 || 1 / x === 1 / y;
        }
        // 针对NaN的情况
        return x !== x && y !== y;
      },
      configurable: true,
      enumerable: false,
      writable: true
    });