---
title: python http.client使用指南
date: 2019-08-15 21:19:51
categories: [python]
tags: [python]
---

## 前言
之所以会有这篇文章是因为之前研究requests校验证书时不校验hostname无法解决，想的一个折中的办法
目前此问题已经解决，如果也有有requests相关的问题，请参考文章[python requests库使用]{https://vinovos.github.io/2019/08/11/python-requests%E5%BA%93%E4%BD%BF%E7%94%A8/}
## 建立连接及请求示例
### 服务器代码
``` python
# coding = utf-8
from flask import Flask, request, jsonify
import ssl
app = Flask(__name__)

cert_path = r'cert'
@app.route("/")
def hello():
    return "Hello World!"

@app.route("/post-test", methods=['POST'])
def post_test():
    data = request.json
    print(data)
    return 'post is ok'

if __name__ == "__main__":
    ssl_context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2)
    # 如果key没有加密，则不用password字段
    ssl_context.load_cert_chain(certfile=r'cert/server.crt', keyfile=r'cert/server-enc.key', password='123456')
    ssl_context.load_verify_locations(r'cert/ca.crt')
    app.run(ssl_context=ssl_context) # 传入ssl_context到flask
```
<!-- more -->
### get以及post请求
```python
import http.client
import ssl,json

headers = { 'Connection': 'close',
            'Content-Type': 'application/json',
            'Host': '127.0.0.1'}
context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2)
context.load_cert_chain(certfile=r'cert/server.crt', keyfile=r'cert/server-enc.key', password='123456')
context.load_verify_locations(r'cert/ca.crt')
# 默认为False
# context.check_hostname = False
h1 = http.client.HTTPSConnection(host='127.0.0.1', port=5000, context=context)
h1.request(method='get', url='/', headers=headers)
rep = h1.getresponse()
print(rep.read(rep.length))
data = {'json':{'name':'vinovos', 'age':'18'}}
data = json.dumps(data)
h1.request(method='post', url='/post-test', headers=headers, body=data)
rep = h1.getresponse()
print(rep.read(rep.length))
h1.close()
```