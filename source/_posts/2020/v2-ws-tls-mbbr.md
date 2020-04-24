---
title: v2+ws+tls+bbr
date: 2020-04-14 21:24:04
categories: ["shadowsocks", "v2ray"]
tags: ["v2ray", "shadowsocks"]
---
#### 安装指南
[参考链接1](http://luyiminggonnabeok.cn/2019/10/18/%E4%B8%80%E9%94%AE%E8%84%9A%E6%9C%AC%E9%85%8D%E7%BD%AEV2ray%E6%A2%AF%E5%AD%90%E6%95%99%E7%A8%8B/)
#### 安装完成之后
### 碰见问题
## 一、没有域名的情况
没有域名，则不要注册域名，因为没有域名的情况也是可以使用的，需要后续更改
## 二、连接时提示证书非信任签名
需要将你生成的根证书在windows上进行安装到可信任根证书
<!-- more -->
## 三、连接时提示证书和host不匹配
这是因为生成ip的证书时，没有在证书里指定ip
### 程序分析
通过安装指南，我们实际上安装的软件有：nginx、v2ray、bbr
其中主要需要额外了解的时nginx和v2ray
## v2ray
v2ray的默认配置文件在/etc/v2ray/config.json
其中设置了一系列的参数，可以根据实际需要进行更改，注意同步修改客户端配置
控制v2ray可以通过systemctl进行start|restart、stop等，将status替换关键字
`systemctl status v2ray`
## nginx
nginx的配置在目录的/etc/nginx/conf和/etc/nginx/conf.d
我们主要关注`/etc/nginx/conf.d/default.conf`
如果没有域名，就把对应的server_name换成你的机器的ip
同时注意ssl_certificate和ssl_certificate_key这两配置项，代表证书和私钥，最好将其替换为自己生成的证书
证书相关问题可以查看博客中的文章<生成CA及终端用户证书>章节
链接 [生成证书](/2019/08/06/openssl使用指南/#生成CA及终端用户证书)
控制nginx，可以直接通过/etc/nginx/sbin/nginx来执行，停止则直接kill掉进程
## bbr
bbr脚本存放于/usr/src/下，tcp.sh就是了，可以根据脚本提示来操作
## 其他服务
其实对于nginx，此脚本创建了一个autov2ray的systemctl控制进程，但是貌似不能正常使用，这里写出来，有兴趣的可以研究下

