---
title: 初探Optional类-代码精进之路(1)
date: 2021-04-26 10:21:55
tags:
- 读后感
- java
- 语法
categories:
- 代码精进之路
---

最近开始读张建飞的《代码精进之路：从码农到工匠》，感觉对于我这样没有开始太多编程的人是一本不错的书：没有经过系统的编码培训，需要从命名规范开始学起。

计划在这个分类下边记录一下读这本书的一些感悟&知识，其实应该是感悟多一些，但是无奈java基础比较差，所以第一篇就以知识开始。

Optional出现在书中的（3.7 精简辅助代码）中，这个看到同事也用过，但是现在还是不了解这个东西可以干嘛，所以就在这里记录一下。

<!--more-->

## 为什么会有Optional？

Optional是java8开始引入的，主要是为了解决一个问题：空指针异常（NullPointerException）

先看看java8之前是这么做的：

以前我们想要完成以下逻辑的时候：

````java
String isocode = user.getAddress().getCountry().getIsocode().toUpperCase();
`````

就会考虑到：中间无论哪一个get如果是null的话，都不会得到最后的结果，所以代码只能这么写：

````java
if (user != null) {
    Address address = user.getAddress();
    if (address != null) {
        Country country = address.getCountry();
        if (country != null) {
            String isocode = country.getIsocode();
            if (isocode != null) {
                isocode = isocode.toUpperCase();
            }
        }
    }
}
````

这样就把代码变得很麻烦。所以java8就推出了Optional来解决这个问题。

## Optioal
