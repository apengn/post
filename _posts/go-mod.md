---
title: go-mod
date: 2019-03-12 09:29:24
tags:
---

https://ieevee.com/tech/2019/02/19/go-mod-proxy.html

https://medium.com/@diogok/on-golang-static-binaries-cross-compiling-and-plugins-1aed33499671


```
GO111MODULE=on go mod init
GOPROXY=https://gocenter.io  GO111MODULE=on go mod vendor


https://goproxy.io
在当前项目下，手动运行go mod tidy
这条命令会自动更新依赖关系，并且将包下载放入cache。在GOPATH/pkg/mod/下。
GOPROXY=https://gocenter.io  GO111MODULE=on go mod tidy
```

docker run -d -p 14000:14000 -p 15002:15002 -p 15001:15001 -p 15003:15003 -p 15004:15004 -p 15000:15000 -v /Users/wupeng/go/src/github.com/uber/kraken/examples/devcluster/config/origin/development.yaml:/etc/kraken/config/origin/development.yaml -v /Users/wupeng/go/src/github.com/uber/kraken/examples/devcluster/config/tracker/development.yaml:/etc/kraken/config/tracker/development.yaml -v /Users/wupeng/go/src/github.com/uber/kraken/examples/devcluster/config/build-index/development.yaml:/etc/kraken/config/build-index/development.yaml -v /Users/wupeng/go/src/github.com/uber/kraken/examples/devcluster/config/proxy/development.yaml:/etc/kraken/config/proxy/development.yaml -v /Users/wupeng/go/src/github.com/uber/kraken/examples/devcluster/herd_param.sh:/etc/kraken/herd_param.sh -v /Users/wupeng/go/src/github.com/uber/kraken/examples/devcluster/herd_start_processes.sh:/etc/kraken/herd_start_processes.sh --name kraken-herd kraken-herd:v0.1.0-25-ga61fa4f ./herd_start_processes.sh




