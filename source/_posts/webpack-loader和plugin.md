---
title: webpack loader和plugin
date: 2022-06-24 10:05:06
tags:
---

#### loader

loader是一个转换器，将A文件进行编译成B文件，比如：A.less转换为A.css，单纯是文件转换过程

#### plugin

plugin是一个扩展器，ta丰富了webpack本身，针对是loader结束后，webpack打包的整个过程，ta并不是直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务