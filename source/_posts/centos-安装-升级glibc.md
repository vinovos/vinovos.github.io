---
title: centos 安装/升级glibc（失败）
date: 2019-08-07 20:13:42
categories: ["centos", "glibc"]
tags: ["centos", "glibc"]
---

``` bash
wget http://ftp.gnu.org/gnu/glibc/glibc-2.23.tar.gz
tar zxvf glibc-2.23.tar.gz
cd glibc-2.23
mkdir build
cd build

../configure --prefix=/opt/glibc-2.23
make -j4
make install

export LD_LIBRARY_PATH=/opt/glibc-2.23/lib
```
<!-- more -->
[Cannot run on CentOS with glibc 2.17](https://github.com/mind/wheels/issues/7)
安装完成之后的压缩包
路径：{% asset_path glibc-2.23-install.tar.gz %}
{% asset_link glibc-2.23-install.tar.gz "编译的压缩包" %}


[https://pkgs.org/](https://pkgs.org/)