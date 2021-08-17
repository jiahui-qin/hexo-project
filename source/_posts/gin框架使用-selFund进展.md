---
title: gin框架使用&selFund进展
date: 2021-08-16 11:59:32
tags:
- gin
- selFund
categories:
- go
---

这里按照时间记录一下gin框架使用的大致问题&如何使用


1. 在gin里使用`get & post`  (2021/8/16)

    get示例：

        router.GET("/getUserFund/:userid", func(c *gin.Context) {
		    userid := c.Param("userid")
		    c.String(http.StatusOK, "Hello %s", userid)
	    })

    使用post取json里的数据相对麻烦一些，需要先创建对应json的结构体，然后再post方法里解析这个结构体，获取里边的数据，这个过程中也可以用gin来做一些校验之类的事情:

        type UserInfo struct {
        	User   string `form:"User" json:"User" xml:"User"  binding:"required"`
        	Fund   string `form:"Fund" json:"Fund" xml:"Fund" binding:"required"`
        	Amount string `form:"Amount" json:"Amount" xml:"Amount" binding:"-"`
        }

	    router.POST("/addUserFund", func(c *gin.Context) {
	    	var json UserInfo
	    	if err := c.ShouldBindBodyWith(&json, binding.JSON); err != nil {
	    		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
	    		return
	    	}
	    	c.String(http.StatusOK, " %s + %s", json.User, json.Fund)
	    })

    关于为什么用`ShouldBindBodyWith`是看了[这篇文章的观点](https://blog.csdn.net/yes169yes123/article/details/106204252),主要是`ShouldBindBodyWith`这个方法保存了requests的body到上下文，允许在处理方法中多次调用body；如果只调用一次的话其实可以用`ShouldBindJSON`来提高效率。

    binding里边可以指定校验方法，比如`required`指定必需，`-`指定不是必填字段等····


2. 引用其他package： (2021/08/17)

    go中引用其他package有一个前提：要在gopath中可以找到，可以用`go env | grep GOPATH`来看一下gopath是哪个文件夹。被引用的包放在这里就比较稳当··虽然感觉怪怪的，但是我的文件目录现在是这样的：

        - GOPATH
            - src
                - main.go
                - go.mod
                - service
                    - user.go
