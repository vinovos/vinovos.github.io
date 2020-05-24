---
title: openssl使用指南（证书)
date: 2019-08-06 21:31:48
categories: ["ssl"]
tags: ["ssl"]
---
### windows安装
[install-openssl-on-windows](https://tecadmin.net/install-openssl-on-windows/)

### 生成并加解密私钥公钥
生成2048位rsa私钥  Generate a 2048 bit RSA Key
`openssl genrsa -out private.key 2048`
生成2048位rsa私钥(aes256加密后) Generate a 2048 bit RSA Key with passphrase
`openssl genrsa -aes256 -out private.key 2048`
导出rsa公钥  Export the RSA Public Key to a File
`openssl rsa -in private.key -outform PEM -pubout -out public.pem`
openssl加密生成后的私钥 add a passphrase
`openssl rsa -aes256 -in private.key -out encrypted.key`
openssl解密加密后的私钥 remove a passphrase
`openssl rsa -in encrypted.key -out private.key`

<!-- more -->
### 生成CA及终端用户证书
* 生成ca证书签发请求，得到ca.csr
私钥和公钥的生成看上面
有配置文件方式：
`openssl req -new -sha256 -out ca.csr -key ca.key -config ca.conf` 
无配置文件方式：
`openssl req -new -sha256 -out ca.csr -key ca.key`
* 生成ca根证书，得到ca.crt(也可以用来做链接，为自签名)
`openssl x509 -req -days 3650 -in ca.csr -signkey ca.key -out ca.crt`
### 生成终端用户证书
* 生成证书签发请求，得到server.csr
`openssl req -new -sha256 -out server.csr -key server.key -config server.conf`
* 用CA证书生成终端用户证书，得到server.crt
`openssl x509 -req -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt -extensions v3_req -extfile server.conf`
ca.conf #证书配置，可以用来生成csr文件，当配置有-config选项
``` conf
[ req ]
default_bits       = 4096
distinguished_name = req_distinguished_name

[ req_distinguished_name ]
countryName                 = Country Name (2 letter code)
countryName_default         = CN
stateOrProvinceName         = State or Province Name (full name)
stateOrProvinceName_default = shanxi
localityName                = Locality Name (eg, city)
localityName_default        = xian
organizationName            = Organization Name (eg, company)
organizationName_default    = vinovos
commonName                  = Common Name (e.g. server FQDN or YOUR name)
commonName_max              = 64
commonName_default          = vinovos CA Test
```
server.conf #证书配置，可以用来生成csr文件，当配置有-config选项
alt_names可以保证请求里面的所有的域名或地址时，都会被认为证书匹配
注意[ alt_names ]的空格
``` ini
[ req ]
default_bits       = 2048
distinguished_name = req_distinguished_name
req_extensions     = v3_req

[ req_distinguished_name ]
countryName                 = Country Name (2 letter code)
countryName_default         = CN
stateOrProvinceName         = State or Province Name (full name)
stateOrProvinceName_default = shanxi
localityName                = Locality Name (eg, city)
localityName_default        = xian
organizationName            = Organization Name (eg, company)
organizationName_default    = vinovos
commonName                  = Common Name (e.g. server FQDN or YOUR name)
commonName_max              = 64
commonName_default          = localhost

[ v3_req ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1   = localhost
IP.1      = 192.168.3.58
IP.2      = 192.168.3.59
IP.3      = 127.0.0.1
```
[ OpenSSL生成CA证书及终端用户证书 ](https://www.cnblogs.com/nidey/p/9041960.html)
### 一句话生成自签名证书和密钥
`openssl req -x509 -newkey rsa:4096 -nodes -out cert.pem -keyout key.pem -days 365`