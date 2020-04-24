---
title: python加解密
date: 2019-08-06 21:29:33
categories: ["python", "加解密"]
tags: ["python", "加解密"]
---
### python读取加密私钥
``` python
from cryptography.hazmat.primitives import serialization
from cryptography.hazmat.backends import default_backend

with open("private.key", "rb") as key_file:
    private_key = serialization.load_pem_private_key(
        key_file.read(),
        password=b'123456',
        backend=default_backend() 
    )
    print(str(private_key))
```
[how-to-decrypt-rsa-encrypted-file-via-php-and-openssl-with-pyopenssl](https://stackoverflow.com/questions/46850665/how-to-decrypt-rsa-encrypted-file-via-php-and-openssl-with-pyopenssl)
<!-- more -->
### python对称加解密(aes cbc)
``` python
import os
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.primitives import padding

padder = padding.PKCS7(128).padder()
unpadder = padding.PKCS7(128).unpadder()
backend = default_backend()
key = os.urandom(32)
iv = os.urandom(16)
cipher = Cipher(algorithms.AES(key), modes.CBC(iv), backend=backend)
encryptor = cipher.encryptor()
data_padder = padder.update(b"a secret message ghhag") + padder.finalize()
ct = encryptor.update(data_padder)
print(ct)
decryptor = cipher.decryptor()
dct = decryptor.update(ct)
data1 = unpadder.update(dct) + unpadder.finalize()
print(data1)
```
[Symmetric encryption](https://cryptography.io/en/latest/hazmat/primitives/symmetric-encryption/)
[Symmetric Padding](https://cryptography.io/en/latest/hazmat/primitives/padding/)

## 中英文对照表
Asymmetric algorithms   非对称加解密
Symmetric encryption    对称加密

