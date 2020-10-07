---
title: java开发项目常用目录结构
categories: []
tags:
  - java
toc: false
date: 2020-10-07 16:00:32
---

# 基于maven的Java项目目录结构
目录结构一般如下所示：
```
${basedir}
        |-- pom.xml
        |-- src
            | |-- main
                | | -- java
                        || com.xxx.xxx 项目源码
                | | `-- resources
                        || 项目配置文件 .xml等
               | | `-- filters
            | | `-- test
                | | `-- java
                | | `-- resources
               | | `-- filters
```
在使用maven来创建的项目，默认会自动创建src/main/等级别的结构。其中各个目录所存放的内容如下：

src/main/java 项目的源代码所在的目录
src/main/resources 项目的资源文件所在的目录
src/main/filters 项目的资源过滤文件所在的目录
src/main/webapp 如果是web项目，则该目录是web应用源代码所在的目录，比如html文件和web.xml等都在该目录下。此目录也通常会放在resources目录下，参见下面的[资源resources文件的结构](#资源resources文件的结构)
src/test/java 测试代码所在的目录
src/test/resources 测试相关的资源文件所在的目录
src/test/filters 测试相关的资源过滤文件所在的目录
# Spring开发项目常用目录结构
## 代码层的结构
1.工程启动类(ApplicationServer.java)推荐放在根目录com.lucky.build包下
2.实体类(domain)
- com.lucky.domain （jpa项目）
- com.lucky.pojo（mybatis项目)
 
3.数据访问层(Dao)
 - com.lucky.repository（jpa项目）
 - com.lucky.mapper（mybatis项目）
 
4.数据服务层(Service)推荐放在com.lucky.service
5.数据服务的实现接口(serviceImpl)放在com.lucky.service.impl
6.前端控制器(Controller) 推荐com.lucky.controller
7.工具类(utils)置于com.lucky.utils
8.常量接口类(constant)置于com.lucky.constant
9.配置信息类(config)置于com.lucky.config
10.数据传输对象（dto）推荐：com.lucky.dto
&emsp;–数据传输对象（Data Transfer Object）用于封装多个实体类（domain）之间的关系，不破坏原有的实体类结构
11.视图包装对象（vo）推荐：com.lucky.vo
&emsp;– 视图包装对象（View Object）用于封装客户端请求的数据，防止部分数据泄露(如:管理员ID)保证数据安全，不破坏原有的实体类结构。
## 资源resources文件的结构
根目录:src/main/resources
1.配置文件(.properties/.json、/resources/application.yml
等)放在config文件夹下
2.静态资源目录：resources/static/
用于存放html、css、js、图片等资源
3.视图模板目录：resources/templates/（用于存放jsp、thymeleaf等模板文件）
4.mybatis映射文件：resources/mapper/（mybatis项目）
5.mybatis配置文件：resources/mapper/config/（mybatis项目）
6.国际化(i18n))置于i18n文件夹下
7.spring.xml置于META-INF/spring文件夹下
8.页面以及js/css/image等置于static文件夹下的各自文件下
# 参考文章
[Spring开发项目常用目录结构](https://blog.csdn.net/weixin_41948075/article/details/103870364)
[基于maven的Java项目目录结构](https://zhuanlan.zhihu.com/p/98456775)