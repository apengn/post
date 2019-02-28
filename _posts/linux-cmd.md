---
title: linux_cmd
date: 2019-02-26 22:02:28
tags:
---

ps -ef |grep {fillter}
kill -9 {pid}

ss -tunlp|grep  7777
ss -alnp |grep 7777
netstat -tunlp|grep 777
netstat -alnp|grep 7777
---------------------
chmod
u 代表用户. 
g 代表用户组. 
o 代表其他. 
a 代表所有.

这意味着chmod u+x somefile 只授予这个文件的所属者执行的权限 
而 chmod +x somefile 和 chmod a+x somefile 是一样的 

// 实现免密登陆
ssh-copy-id root@hostip


### 压缩文件
tar -czvf wisecloud-ingress-controller.gz ./wisecloud-ingress-controller
### 解压文件
tar -zxvf wisecloud-ingress-controller.gz -o ./


docker run --rm -it golang:1.10 sh
```
grep -A 50

grep -B 50
``

