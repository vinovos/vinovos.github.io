---
title: python中json使用指南
date: 2019-08-25 10:33:18
categories: [python, json]
tags: [python, json]
---
## 前言及前置知识
随着restful接口的兴起，json格式代替xml进行http交互使用越来越普遍，json比起xml更适合人来使用。
### json格式
通常json格式由json字符串或json数组组成，可以互相包含
json字符串如下
``` json
{
    "name" : "vinovos",
    "age" : "18"
}
```
> 可以看出是字典数据结构
json数组如下
``` json
[
    {
        "name" : "vinovos"
    },
    {
        "name" : "sovoniv"
    }
]
```
> 可以看出数组里面包含了json字符串，当然也可以包含数组
<!-- more -->
### json格式与python中数据结构的异同
json格式由典型的字典与数组格式组成，这与python中的数据结构相通，但是有如下几个区别：
1. json格式中只能使用双引号"，而不能使用',python中则都可以
2. json中当字典值为true或false时，用true/false表示，而python中则为True/False
3. json中当字典值为空时，用null表示，而python中用None

> note：这些差异点的不同，可以让你在转换json时更加得心应手，而不知道错误在哪
## json格式的操作-python
在python中对json的操作是通过json对象来实现的，使用前需要
`import json`
python对json的使用，我认为可以分为三部分：生成json对象，使用json对象，导出/转换json对象
json库使用的设计方案是基于序列化和反序列化的。
### 生成json对象
生成json对象可以通过load的一系列函数实现，通常json对象的来源有两个：字符串和文件，分别使用如下：
字符串: `json_var = json.loads(str)`
文件: `json_var = json.load(f)`
### 使用json对象
使用json对象的时候，就如同使用python自己的数据结构一样，根据对象的实际结构调用
json字典: `json_var['key']`
json数组: `json_var[0]`    有需要时，可以通过`for i in json_var`来遍历数组
json数组包含字典：`json_var[0]['key'`
### 导出/转换json对象
在python中可以通过dump的系列函数，将json转换为字符串或文件中
字符串：`str = json.dumps(json_var)`
写入文件：`json.dump(json_var,f)`
