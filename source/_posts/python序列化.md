---
title: python序列化
date: 2019-08-25 19:34:42
categories: [python, 序列化]
tags: [python, 序列化]
---
序列化和反序列化是一个非常重要的特性，当你想保存对象到文件，读取配置文件，发送http请求，你就会用到序列化和反序列化。
但是序列化是一个比较麻烦的事情，你并不想关注格式和协议，而仅仅想保存对象，然后晚点完整的恢复它。
当你使用序列化时，你的序列化方案取决去运行速度、安全、便捷以及和其他程序的交互。
## 运行示例（基础结构）
在最后面的示例中，我们都会使用这个基础结构来进行序列化和反序列化
简单的对象：
``` python
simple = dict(int_list=[1, 2, 3], text='string', number=3.44, boolean=True, none=None)
```
复杂的对象：
``` python
from datetime import datetime
class A(object):
    def __init__(self, simple):
        self.simple = simple

    def __eq__(self, other):
        if not hasattr(other, 'simple'):
            return False
        return self.simple == other.simple
    
complex = dict(a=A(simple), when=datetime(2016, 3, 7))
```
<!-- more -->
## Pickle
pickle时python自带的一个序列化和反序列化格式库。提供了四种方法： dump, dumps, load, loads
> load操作文件，loads操作字符串，dump操作文件，dumps操作字符串，后面不在列出
序列化及反序列化代码：
``` python
import pickle
simple_serialize = pickle.dumps(simple)
simple_serialize_highest = pickle.dumps(simple, protocol=pickle.HIGHEST_PROTOCOL)
simple = pickle.loads(simple_serialize_highest)
pickle.dump(simple, open('simple', 'wb'), protocol=pickle.HIGHEST_PROTOCOL)
```
## JSON
**如果不是必须的情况下，不建议使用json来将非json格式对象来序列化**
json格式的基本操作见 {% post_link python中json使用指南 [python中json使用指南] %}
这里我们来操作复杂的对象（默认失败）
```python
import json
json.dumps(complex)
--------------------------------------------------------------------------------------------------
    json.dumps(complex)
  File "c:\program files (x86)\python37\Lib\json\__init__.py", line 231, in dumps
    return _default_encoder.encode(obj)
  File "c:\program files (x86)\python37\Lib\json\encoder.py", line 199, in encode
    chunks = self.iterencode(o, _one_shot=True)
  File "c:\program files (x86)\python37\Lib\json\encoder.py", line 257, in iterencode
    return _iterencode(o, 0)
  File "c:\program files (x86)\python37\Lib\json\encoder.py", line 179, in default
    raise TypeError(f'Object of type {o.__class__.__name__} '
TypeError: Object of type A is not JSON serializable
```
可以看到，如果直接dumps会报错，提示A不是一个json序列化，从而无法序列化
> json是一个格式限制非常严格的类型，不能自动序列化用户自定义的类
要解决以上的问题，需要创建一个JSONEncoder子类并实现`default()`，这个函数会在json无法自己实现序列化的时候调用
因为json无法自己序列化的都需要我们自己实现，所以我们在complex中需要实现A和datetime的序列化
```python
class CustomEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, datetime):
            return {'__datetime__': o.replace(microsecond=0).isoformat()}
        return {'__{}__'.format(o.__class__.__name__): o.__dict__}
json.dumps(complex, indent=4, cls=CustomEncoder)
```
在上面的代码中，我们将自定义的类A和datetime转换成字典返回，key值为`__classname__`的格式，当然key可以自己定义
通过上面的代码，我们成功的将complex序列化，但是当反序列化的时候，则发现反序列化的结果与最原始的complex不同
```python
complex_json = json.dumps(complex, indent=4, cls=CustomEncoder)
complex_json_deserialize = json.loads(complex_json)
assert complex == complex_json_deserialize
```
运行上面的代码，我们会发现AssertionError
> 我们可以通过pprint打印出对象来观察。会发现当我们自定义encoder函数的时候，decode之后会和encoder函数有关系
> 自定义encode函数会导致和原始对象的不同，这时候我们需要自定义decode函数
我们这次不需要定义一个自定义decoder子类，load()和loads()函数提供了"object_hook"参数来提供自定义的函数
```python
def decode_custom(obj):
    if '__A__' in obj:
        a = A(None)
        a.__dict__.update(obj['__A__'])
        return a
    elif '__datetime__' in obj:
        return datetime.strptime(obj['__datetime__'], '%Y-%m-%dT%H:%M:%S')
    return obj
complex_json_deserialize = json.loads(complex_json, object_hook=decode_custom)
assert complex == complex_json_deserialize
```
如上所示：我们将我们自定`__classname__`，转换成对应的对象返回，则解析成功，
## YAML
[YAML](https://yaml.org/)是一个对人比较友好的数据序列号格式，srping-boot中也支持此格式的配置
在python中则需要第三方库来支持使用yaml格式,这里推荐两个
`pip install PyYAML` 较为活跃
`pip install aspy.yaml`对PyYAML做了一些扩展，有特殊需要可以考虑
PyYAML只含有load() and dump()两个函数
PyYAML在最新的版本中，因为安全原因对原来的代码代行了调整，建议参数设置Loader，具体Loader有：
[PyYAML yaml.load(input) Deprecation](https://github.com/yaml/pyyaml/wiki/PyYAML-yaml.load\(input\)-Deprecation#footnotes)
``` python
import yaml
simple_serialize = yaml.dump(simple)
simple = yaml.load(simple_serialize)
yaml.dump(simple, open('simple.yml', 'w'))
simple = yaml.load(open('simple.yml'), Loader=yaml.BaseLoader)
```
## 性能
//TODO
## 安全
> Pickle和YAML都可以把数据直接转化为对象，所以不要将这两个函数用于不信任或者未验证的代码
> Never unpickle data received from an untrusted or unauthenticated source.
PyYAML提供了SafeLoader来进行安全防护，如果要进行不信任输入数据进行序列化，YAMl的Loader请使用此参数
## 其他序列化格式
**Protobuf**
protobuf是google团队开发的用于高效存储和读取结构化数据的工具，是高效的序列化数据结构的协议. 使用C++实现，但是有python库. 是一种比较复杂，但是强大的数据格式.

**MessagePack**
MessagePack是另一种比较流行的数据序列化格式(serialization format). 它也是二进制并且高效,但它不像Protobuf需要一种数据格式(schema)。使用了一种比较类似于JSOn的类型系统(type system),但是更加丰富。keys支持除了strings和non-utf strings的其他格式

**CBOR**
CBOR是一种提供良好压缩性，扩展性强，不需要进行版本协商的二进制数据交换形式.它支持JSON数据模型，不如Protobuf和MessagePack出名，但是比较有趣，因为下面两个原因：
* 有比较官方的支持RFC 7049
* 它被特别设计用来针对物联网(IoT)

## 如何选择使用哪种格式
请根据实际情况选择合适的序列化协议，一般来说选择需要考虑下面四个方面：
    1、序列化后的格式是否人类可读、可编辑的
    2、被序列化内容是否会来自于不被信任的输入
    3、是否有高性能的需求或性能瓶颈
    4、是否需要与非python环境交互
### 自动保存本地状态在python程序中
采用使用HIGHEST_PROTOCOL参数的pickle模块。它是非常快速、高效并且存储大多数的python对象而不需要额外的操作
### 配置文件
使用YAML格式。YAML被ansible和sping boot使用，是一个对人很友好的格式。
### Web APIS
JSON是一个常用的Web apis接口所使用的格式，JSON格式是JavaScript的默认类型，适合前后端交互。
### 大规模、低延迟交互
使用二进制协议中的一种：Protobuf（如果需要一种格式定义），MessagePack或者CBOR

## 参考链接
[Serialization and Deserialization of Python Objects: Part 1](https://code.tutsplus.com/tutorials/serialization-and-deserialization-of-python-objects-part-1--cms-26183)
[Serialization and Deserialization of Python Objects: Part 2](https://code.tutsplus.com/tutorials/serialization-and-deserialization-of-python-objects-part-2--cms-26184)