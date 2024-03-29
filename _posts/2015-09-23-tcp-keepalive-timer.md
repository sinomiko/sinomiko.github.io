---
published: true
layout: post
title: TCP 之保活计时器
category: network
tags: 
  - tcp
time: 2016.02.9 14:22:00
excerpt: 保活计时器并不是 TCP 规范中的一部分，是一个有争论的功能。
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---



保活计时器并不是 TCP 规范中的一部分，是一个有争论的功能。

<!--more-->

如果一个给定的连接在两个小时之内没有任何动作，则服务器就向客户发送一个探查报文段，客户机有如下状态：

 > 1. 客户机仍正常运行，回复响应。服务器在两小时以后将保活定时器复位。
 > 2. 客户机已崩溃，TCP 都没有响应，75秒后超时。服务器总共发10个这样的探查，每个间隔75秒。
 > 3. 客户机已重新启动，客户机回复 RST。
 > 4. 客户机正常运行，但是服务不可到达，与状态2相同。
