---
title: mvn常用命令积累
date: 2021-02-26 15:28:50
tags:
- mvn
categories:
- 初学者教程
---

# 什么是MVN?

<!--more-->
# 常用命令

## 打包跳过测试

* mvn clean install -DskipTests
    
    * 不执行测试用例，但是编译测试用例生成相应class文件到target/test-classes下

* mvn clean install -Dmaven.test.skip=true
    
    * 不执行测试用例，也不编译测试用例的类
