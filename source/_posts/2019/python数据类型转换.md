---
title: python数据类型转换
date: 2019-08-27 21:41:07
categories: [python]
tags: [python]
---
我们在这里只讨论基本的数据类型转换，默认的情况下包含
`strings, bytes, numbers, tuples, lists, dicts, sets, booleans, None`
## 字符串转换
### 字符串转元组
字符串转元组可以分为三类，本来时元组转的字符串，普通字符串转为元组单个元素，其他类型的
#### 元组字符串转元组
使用ast.literal_eval转换
> 也可以eval直接转换，但是eval存在安全风险，literal_eval只会执行转换Python literal structures
> Python literal structures: strings, bytes, numbers, tuples, lists, dicts, sets, booleans, and None
```python
import ast
str_tuple = '(1, 2, 3)'
# str_tuple = '1, 2, 3'
tuple_str = ast.literal_eval(str_tuple)
```
#### 普通字符串转元组单个元素
直接通过(arg,)的方式转换，主力里面的逗号(,)
```python
str1 = 'tuple'
tuple_str = (str1,)
```
#### 其他类型字符串转元组
通过先转为列表，再转为元组
```python
str1 = '1 2 3'
tuple_str = tuple(str.split())
```