---
title: 如何展示quartz所有可执行job
date: 2021-11-09 17:29:25
tags:
- quartz
- java
categories:
- devlop
---

最近在写一个定时任务系统，其实是相当简单，也就是用了quartz，但是写的时候遇到了一个小问题，前端创建scheduler的时候，总不能传待执行task的类名吧，这也太尬了。于是就又有了一个小小的需求：要展示所有可执行task的名称

在我的代码里，我的所有可执行task都提供了一个接口，在一个quartz模块里有一个package里边写了很多个类就专门来调task接口，比如：

```java

/job/job001.java

public class job001 implements Job {

    @Override
    public void execute(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        log.info("do job001");
    }
}

```

于是就在job这个package下所有的类就全部都是可供执行的job。那么这个任务就等价于：如何展示这个package下所有class的类名和名称

其实最简单的方法：就是搞上一个数据库，把数据都存进去，可是这样未免过于不优雅，每次新增任务的时候还要更新数据库里边的数据。于是我就想：能不能直接操作文件？

于是搜了一番果然找到有大佬早早就做过类似的工作：大佬做的工作也非常详细，直接提供类方法可以查看jar包和file里的所有类名，一番删除修改之后就直接可以获取到这个package下所有的类名了，可以说是非常方便

那么还有一个问题：如何给这个类名对应一个便于前端理解的名字呢？我就给每个task类都加了一个`getName()`的方法，使用反射来调用这个方法，就可以获取到类所对应的名称了。一般而言，这里想要都是有其固定意义的，想要写重复还是挺难的把！再说就算写重复也还是可以校验的。