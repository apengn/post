---
title:  证书制作
date: 2019-06-23 22:53:42
tags:
---
```
#!/bin/bash

# step 1 为服务器端和客户端准备公钥、私钥
# 生成服务器端私钥
openssl genrsa -out server.key 1024
# 生成服务器端公钥
openssl rsa -in server.key -pubout -out server.pem


# step 2 生成 CA 证书
# 生成客户端私钥
openssl genrsa -out client.key 1024
# 生成客户端公钥
openssl rsa -in client.key -pubout -out client.pem

# 生成 CA 私钥
openssl genrsa -out ca.key 1024
# X.509 Certificate Signing Request (CSR) Management.
openssl req -new -key ca.key -out ca.csr
# X.509 Certificate Data Management.
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt

# ➜  keys  openssl req -new -key ca.key -out ca.csr
# You are about to be asked to enter information that will be incorporated
# into your certificate request.
# What you are about to enter is what is called a Distinguished Name or a DN.
# There are quite a few fields but you can leave some blank
# For some fields there will be a default value,
# If you enter '.', the field will be left blank.
# -----
# Country Name (2 letter code) [AU]:CN
# State or Province Name (full name) [Some-State]:Zhejiang
# Locality Name (eg, city) []:Hangzhou
# Organization Name (eg, company) [Internet Widgits Pty Ltd]:My CA
# Organizational Unit Name (eg, section) []:
# Common Name (e.g. server FQDN or YOUR name) []:localhost
# Email Address []:


# step 3 生成服务器端证书和客户端证书
# 服务器端需要向 CA 机构申请签名证书，在申请签名证书之前依然是创建自己的 CSR 文件
openssl req -new -key server.key -out server.csr
# 向自己的 CA 机构申请证书，签名过程需要 CA 的证书和私钥参与，最终颁发一个带有 CA 签名的证书
openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt

# client 端
openssl req -new -key client.key -out client.csr
# client 端到 CA 签名
openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in client.csr -out client.crt


获取服务端签名, 即server.crt的内容:
openssl x509 -in <(openssl s_client -showcerts -servername dfdaemon.com -connect dfdaemon.com:65001 -prexit 2>/dev/null)
```

```
openssl x509部分命令
打印出证书的内容：
openssl x509 -in cert.pem -noout -text
打印出证书的系列号
openssl x509 -in cert.pem -noout -serial
打印出证书的拥有者名字
openssl x509 -in cert.pem -noout -subject
以RFC2253规定的格式打印出证书的拥有者名字
openssl x509 -in cert.pem -noout -subject -nameopt RFC2253
在支持UTF8的终端一行过打印出证书的拥有者名字
openssl x509 -in cert.pem -noout -subject -nameopt oneline -nameopt -escmsb
打印出证书的MD5特征参数
openssl x509 -in cert.pem -noout -fingerprint
打印出证书的SHA特征参数
openssl x509 -sha1 -in cert.pem -noout -fingerprint
把PEM格式的证书转化成DER格式
openssl x509 -in cert.pem -inform PEM -out cert.der -outform DER
把一个证书转化成CSR
openssl x509 -x509toreq -in cert.pem -out req.pem -signkey key.pem
给一个CSR进行处理，颁发字签名证书，增加CA扩展项
openssl x509 -req -in careq.pem -extfile openssl.cnf -extensions v3_ca -signkey key.pem -out cacert.pem
给一个CSR签名，增加用户证书扩展项
openssl x509 -req -in req.pem -extfile openssl.cnf -extensions v3_usr -CA cacert.pem -CAkey key.pem -CAcreateserial
```