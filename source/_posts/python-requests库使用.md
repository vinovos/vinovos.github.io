---
title: python requests库使用
date: 2019-08-11 20:16:52
categories: [python, requests]
tags: [python, requests]
---
[官方文档](https://2.python-requests.org/en/master/)
### 基础用法

### requests支持客户端私钥加密 requests support client encrypt key
### requests支持不验证证书的hostname
``` python
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.ssl_ import create_urllib3_context
from urllib3.poolmanager import PoolManager
import ssl

class SSLAdapter(HTTPAdapter):
    '''
    使用方法，见下面的请求方式
    目的： 自定义https请求方式，可以使用自定义证书和密钥，支持密钥加密
    note: 目前实现的功能不支持校验hostname,如果要校验，请将assert_hostname更改为True，或者删除
    '''
    def __init__(self, certfile, keyfile, password=None, *args, **kwargs):
        self._certfile = certfile
        self._keyfile = keyfile
        self._password = password
        # 如果要校验hostnae，则设置为True
        self.assert_hostname = False
        return super(self.__class__, self).__init__(*args, **kwargs)

    def init_poolmanager(self, *args, **kwargs):
        self._add_ssl_context(kwargs)
        return super(self.__class__, self).init_poolmanager(*args, **kwargs)

    # init_poolmanager另一种实现方式
    # def init_poolmanager(self, connections, maxsize, block=False):
    #     self.poolmanager = PoolManager(num_pools=connections,
    #                                    maxsize=maxsize,
    #                                    block=block,
    #                                    assert_hostname=self.assert_hostname,
    #                                    ssl_context=self._add_ssl_context()
    #                                    )


    def proxy_manager_for(self, *args, **kwargs):
        self._add_ssl_context(kwargs)
        return super(self.__class__, self).proxy_manager_for(*args, **kwargs)

    def _add_ssl_context(self, kwargs=None):
        context = create_urllib3_context()
        context.load_cert_chain(certfile=self._certfile,
                                keyfile=self._keyfile,
                                password=str(self._password))
        if kwargs is not None:
            kwargs['ssl_context'] = context
            kwargs['assert_hostname'] = self.assert_hostname
        return context

session = requests.Session()
session.mount('https://', SSLAdapter(r'cert/client.crt', r"cert/client-enc.key", '123456'))
r = session.get('https://127.0.0.1:5000', verify=r'cert/ca.crt')
print(session.get_adapter('https://127.0.0.1:5000'))
print(r.text)
```
[Private Encrypted Key Workaround with Requests](https://stackoverflow.com/questions/51002776/private-encrypted-key-workaround-with-requests-ca-certificate)