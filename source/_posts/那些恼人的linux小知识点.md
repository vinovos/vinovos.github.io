---
title: 那些恼人的linux小知识点
date: 2019-08-14 20:19:58
categories: linux
tags: linux
---
### 1、如. .bashrc命令里面的.代表什么
> .代表的其实是命令source命令，上面的命令等同与source .bashrc
### 2、copy命令里面copy src/* dest是否真的复制了全部文件过去
> copy src/* dest只会复制src目录下的可见文件，而不会复制隐藏文件，要同时复制隐藏文件请使用copy src/. dest
### 3、在执行了一条命令(/usr/bin下的make)，然后删掉了/usr/bin下的make,存在/bin/make,再次执行make，会提示不存在/usr/bin/make，什么原因
> linux在一个session里面会记录执行过的命令，并记录成hash表，下次直接读取hash，如果删掉了存在的命令文件，就会提示，这时可以执行hash -r命令来刷新hash表
<!-- more -->
### 4、使用if [ -n $abcde]，abcde变量不存在，为什么会判定为if true
> 使用if -n判定的是字符串是否长度不为0或者不为空，当直接使用$abcde时，会判定为true，如果想检测是否$abcde存在，则应该使用if [ -n "$abcde" ]，在为变量加上""，另外if [[ -n $abcde ]]也会正确的判定为false。[[]]与[]的区别时[[]]不会根据空格等进行分割变量，另外也不会展开*等
