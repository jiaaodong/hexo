---
title: 如何在Linux Server中使用多个terminal
date: 2018-11-04 17:01:30
tags:
---
- 通过`screen`来实现用一个ssh连接到server后打开多个terminal。
[`screen`的使用](https://help.ubuntu.com/community/Screen)
- 通过多个ssh连接到server。
  [关于是否可以这样做的解释](https://www.howtogeek.com/240809/is-it-possible-to-have-multiple-ssh-connections-to-the-same-system/)
  * The Short Answer:
    >Yes, it usually works by default.
  * The Long Answer:
    >It depends on what you are using it for. It may slow down with multiple connections, but that is a bandwidth issue, not an SSH issue."
