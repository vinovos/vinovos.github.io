---
title: spring cloud 入门到实战-楠哥B站视频
categories:
  - java
  - springcloud
tags:
  - java
  - springcloud
toc: false
date: 2020-05-24 16:15:29
---

这两天在B站学习了下spring cloud的使用
B站的学习视频：[视频链接](https://www.bilibili.com/video/BV1p4411K7pz?p=11)
网友们写的学习笔记：[笔记链接](https://my.oschina.net/qq785482254/blog/3217701)
另一个网友写的笔记：pdf或者md格式，提取码`21ru` [网盘链接](https://pan.baidu.com/s/1o0Aju3IydKA15Vo1pP4z5w)
以下是我个人使用过程的一些情况：
在实战中部分依赖等可能与作者并不相同
### 实战代码地址
[我的仓库代码地址]()
### 教程评价
非常简单的应用指南，基本全程在创建子模块，然后启动，演示
作用：适合跟着快速的了解一下spring cloud，实际应用需要更多的学习等
### idea插件
- Lombok
- Spring Assistant
### Eureka
eureka在实际测的过程中没有像视频中显示为down，而是被移除
### Zipkin
zipkin无法创建server端，经过一系列查询，无法解决问题。
在官网上，发现作者也不支撑自己构建server，而是使用现有的release版本
这个是网友提交的相关issue以及回复 [issue地址](https://github.com/openzipkin/zipkin/issues/2930)
这个是官网的声明链接 [官网声明](https://github.com/openzipkin/zipkin/blob/master/zipkin-server/README.md#custom-servers-are-not-supported)
看B站评论区的网友说可以去除依赖,未验证
```
	<dependency>
            <groupId>io.zipkin.java</groupId>
            <artifactId>zipkin-server</artifactId>
            <version>2.9.4</version>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.logging.log4j</groupId>
                    <artifactId>log4j-slf4j-impl</artifactId>
                </exclusion>
            </exclusions>
	</dependency>
```