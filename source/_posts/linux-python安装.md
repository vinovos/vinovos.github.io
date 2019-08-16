---
title: linux python安装
date: 2019-08-07 22:08:37
categories: ["linux", "python"]
tags: ["linux", "python"]
---
## 编译安装
如果官方没有rpm或者没有其他提供的rpm包，则需要自己编译（一般是最新版本)
### python3.7

```
wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
yum -y install gcc openssl-devel bzip2-devel libffi-devel
tar xzf Python-3.7.4.tgz
cd Python-3.7.4
./configure --prefix=/opt/python37
make && make install
```
