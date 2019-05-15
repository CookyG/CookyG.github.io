---
title: calc less写法
url: 99.html
id: 99
categories:
  - css
date: 2018-02-23 13:46:00
tags:
---

div { @diff : 30px; width : calc(~"100% - @{diff}"); } 这种写法又能编译，Webstorm里又不报错，所以我比较喜欢用这种写法，如此，便不会再有任何问题了。