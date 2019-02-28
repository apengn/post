---
title: helm
date: 2019-02-26 18:42:41
tags:
---


#### helm  注意事项

helm  install chats 与repo 有关

helm 获取删除chats release 只与config 有关

helm repo add  可以 重复，第二次添加类似update

helm package wp --debug

### 启动helm repo  serve
```
nohup ./helm serve --address 0.0.0.0:8879 &
```

### 通过harbor 启动helm repo
```
sudo ./install.sh   --with-clair --with-chartmuseum
```
### 添加harbor repo
```
 helm repo add --username=admin --password=Passw0rd myrepo https://xx.xx.xx.xx/chartrepo
```
### 添加特定仓库
```
   helm repo add --username=admin --password=Passw0rd myrepo https://xx.xx.xx.xx/chartrepo/myproject
```
