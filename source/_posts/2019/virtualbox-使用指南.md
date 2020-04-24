---
title: virtualbox 使用指南
date: 2019-08-07 20:49:23
categories: "virtualbox"
tags: "virtualbox"
---

## 安装增强功能
虚拟机系统:centos7
### 出现错误
安装完成操作系统之后，之后点击设备，安装增强功能会报错
提示 > 未能加载虚拟光盘(路径\\VBoxGuestAdditions.iso)
解决办法：点击“设备”——“分配光驱”——“移除光盘”，将自动加载的光盘去掉,然后重新点击安装增强功能
### 准备工作
`dhclient` 命令，在dbox配置好网络之后，使用此命令来让centos7获取ip地址
使用yum update的目的是为了可以安装到对应的kernel-devel
<!-- more -->
```
yum update
yum install gcc
yum install kernel-devel-2.6.32-504.el6.x86_64
yum install bzip2
```
notes: yum update之后可能会升级内核，会让系统中出现两个内核，此时需要重启，并使用uname -r确定是否使用了新内核
  如果使用了新内核，那么旧内核可以通过yum remove 来移除
### 安装步骤
``` bash
mkdir /mnt/cdrom
mount /dev/sr0 /mnt/cdrom
/mnt/cdrom/runasroot.sh
VboxLinuxAddtions.run
```