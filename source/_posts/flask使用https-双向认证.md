---
title: flask/python使用https(双向认证)
date: 2019-08-11 17:12:55
categories: ["python", "flask", "https"]
tags: ["python", "flask", "https"]
---
### flask使用https
可以看到主要通过对ssl_context进行赋值，来启用https
``` python
from flask import Flask
app = Flask(__name__)

cert_path = r'cert'
@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(ssl_context=(r'cert/server.crt', r'cert/server.key'))
```
<!-- more -->
### 服务器启用双向认证(只列出核心代码)
对可以接受SSLContext类型参数的https链接，都可以使用如下代码
``` python
    ssl_context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2)
    # 如果key没有加密，则不用password字段
    ssl_context.load_cert_chain(certfile=r'cert/server.crt', keyfile=r'cert/server-enc.key', password='123456')
    ssl_context.load_verify_locations(r'cert/ca.crt')
    ssl_context.verify_mode=ssl.CERT_REQUIRED
    # 本来以为这个verify_flags是校验参数，后面发现应该是校验当前证书是否可用，如未损坏等
    #ssl_context.verify_flags=ssl.VERIFY_CRL_CHECK_LEAF
    # app.run(ssl_context=ssl_context) # 传入ssl_context到flask
```
[python ssl库文档](https://docs.python.org/3/library/ssl.html?highlight=sslcontext)
### 测试代码
``` python
import requests
r = requests.get('https://127.0.0.1:5000/', verify=r'cert/ca.crt', cert=(r'cert/client.crt', r'cert/client.key'))
print(r.text)
```
{% post_link python-requests库使用 参考客户端密钥加密的请求 %}