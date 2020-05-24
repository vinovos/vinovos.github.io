---
title: java证书校验
categories:
  - java
  - ssl
tags:
  - ssl
  - java
toc: false
date: 2020-04-27 21:07:32
---

### 参考链接
[HTTPS 精读之 TLS 证书校验](https://zhuanlan.zhihu.com/p/30655259)
[HttpComponents](https://hc.apache.org/httpcomponents-client-ga/)
### 总述
因为java自身并没有提供良好的http请求接口，所以我们一般使用httpclient来进行http/https请求。在进行https请求的时候，httpclient默认会对证书进行校验，如果不符合要求则会断开连接。常见的校验有证书签名、证书有效期、证书CN匹配等。但是因为某些原因，我们的证书并不完全符合默认的要求(如私有证书)，这时候我们就需要对证书校验做一些自定义。在这篇文章中，我们会去实现自定义校验，并提供进行某项特殊校验的方法。
### 自定义ssl校验
我们可以在httpclient官网上面的样例中，找到自定义ssl context的部分，里面提供了示例代码
https://hc.apache.org/httpcomponents-client-ga/httpclient/examples/org/apache/http/examples/client/ClientCustomSSL.java
我们来分析下这个代码,首先在pom中添加依赖
<!-- more -->
```
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.12</version>
</dependency>
```
![image.png](/images/2020/05/03/0e64d4f0-8ce5-11ea-8eb1-072a22f8a6db.png)
如图，创建一个httpclient分为三步，而我们可以实际操作ssl验证的是前两步。
### 信任所有证书
在代码中将步骤1的`new TrustSelfSignedStrategy()`替换为`new TrustAllStrategy()`
### 校验hostname
默认校验所有hostname(包含CN和SAN)
SAN：Subject Alt Name
CN：Subject's Name中的CN字段（证书使用者）
![image.png](/images/2020/05/03/242a9db0-8cea-11ea-8eb1-072a22f8a6db.png)
#### 不校验hostname
在代码中将步骤1的`SSLConnectionSocketFactory.getDefaultHostnameVerifier()`替换为`new NoopHostnameVerifier()`
### 更加个性化的校验
我们在上面的可以看到通过更改代码中的参数，来实现我们所需要的操作，但是提供的参数有限，一些情况下我们需要更深入的自定义校验操作。
让我们来重新回顾下，上面的代码，共分为三个步骤，23步骤都是httpclient的相关操作，而步骤一则是创建sslcontext，虽然此处使用了httpclient提供的创建sslcontext的操作，但是实际上在java中，我们也可以通过其他的办法来创建sslContxt；同时，在使用httpclient的相关操作时，我们可以给其提供更为强大的功能（基于其装饰者模式？）
