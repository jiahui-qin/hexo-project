---
title: Jsch 初应用
date: 2021-03-04 17:25:01
tags:
- java
- jsch
categories:
- java初学者教程
---

## 什么是Jsch

Jsch是一个纯java写的ssh客户端，通过Jsch可以完全通过java来实现ssh的一些功能

## 为什么要用Jsch？

有个需求要做一个netconf长连接到设备上，监听notifaction，看了一圈好像没有现成的解决办法，所以只能自己想办法研究Jsch写一个

<!--more-->

## Jsch探索

### Jsch中连接的方式：

在Jsch中一个ssh连接称为一个Session，在一个建立好的Session中可以用 `sshSession.openChannel`获取不同类型的Channel：

|channel|用途|备注
|--|--|--|
|exec|单独执行一个命令
|shell|远程终端方式交互
|subsystem| 子系统|要用`setSubsystem`来指定子系统类型，比如cmd，netconf
|sftp|传输文件

### 获取ssh的输出

对channel有一个getInputStream的方法，可以获取的输出信息流，输出的类型是OutPutStream
