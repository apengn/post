---
title: https_harbor 搭建
date: 2019-06-25 18:19:53
tags:
---

参考：https://github.com/goharbor/harbor/blob/master/docs/configure_https.md
```
openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -sha512 -days 3650 \
    -subj "/C=TW/ST=Taipei/L=Taipei/O=example/OU=Personal/CN=yourdomain.com" \
    -key ca.key \
    -out ca.crt
openssl genrsa -out yourdomain.com.key 4096

openssl req -sha512 -new \
    -subj "/C=TW/ST=Taipei/L=Taipei/O=example/OU=Personal/CN=yourdomain.com" \
    -key yourdomain.com.key \
    -out yourdomain.com.csr
cat > v3.ext <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[alt_names]
DNS.1=yourdomain.com
DNS.2=yourdomain
DNS.3=https-harbor
DNS.4=10.0.0.88
EOF
openssl x509 -req -sha512 -days 3650 \
    -extfile v3.ext \
    -CA ca.crt -CAkey ca.key -CAcreateserial \
    -in yourdomain.com.csr \
    -out yourdomain.com.crt

mkdir -p /data/cert/
cp yourdomain.com.crt /data/cert/
cp yourdomain.com.key /data/cert/

openssl x509 -inform PEM -in yourdomain.com.crt -out yourdomain.com.cert

mkdir -p /etc/docker/certs.d/yourdomain.com/
cp yourdomain.com.cert /etc/docker/certs.d/yourdomain.com/
cp yourdomain.com.key /etc/docker/certs.d/yourdomain.com/
cp ca.crt /etc/docker/certs.d/yourdomain.com/
```