---
title: dragonfly 之 p2p 镜像分发 
date: 2019-02-26 09:07:21
tags:
---
支持https harbor，公有或私有仓库
支持push image 

### 什么是dragonfly
 Dragonfly 是一款基于 P2P 的智能镜像和文件分发工具。它旨在提高文件传输的效率和速率，最大限度地利用网络带宽，尤其是在分发大量数据时，例如应用分发、缓存分发、日志分发和镜像分发。

在阿里巴巴，Dragonfly 每个月会被调用 20 亿次，分发的数据量高达 3.4PB。Dragonfly 已成为阿里巴巴基础设施中的重要一环。

尽管容器技术大部分时候简化了运维工作，但是它也带来了一些挑战：例如镜像分发的效率问题，尤其是必须在多个主机上复制镜像分发时。

Dragonfly 在这种场景下能够完美支持 Docker 和 PouchContainer。它也兼容其他格式的容器。相比原生方式，它能将容器分发速度提高 57 倍，并让 Registry 网络出口流量降低 99.5%。

Dragonfly 能让所有类型的文件、镜像或数据分发变得简单而经济。
### Dragonfly 有何优势（具备以下特性）？
- 基于 P2P 的文件分发：通过利用 P2P 技术进行文件传输，它能最大限度地利用每个对等节点（Peer）的带宽资源，以提高下载效率，并节省大量跨机房带宽，尤其是昂贵的跨境带宽。
- 非侵入式支持所有类型的容器技术：Dragonfly 可无缝支持多种容器用于分发镜像。
- 机器级别的限速：除了像许多其他下载工具（例如 wget 和 curl）那样的针对当前下载任务的限速之外，Dragonfly 还支持针对整个机器的限速。
- 被动式 CDN：这种 CDN 机制可防止重复远程下载。
- 高度一致性：Dragonfly 可确保所有下载的文件是一致的，即使用户不提供任何检查代码（MD5）。
- 磁盘保护和高效 IO：预检磁盘空间、延迟同步、以最佳顺序写文件分块、隔离网络-读/磁盘-写等等。
- 高性能：SuperNode 是完全闭环的，意味着它不依赖任何数据库或分布式缓存，能够以极高性能处理请求。
- 自动隔离异常：Dragonfly 会自动隔离异常节点（对等节点或 SuperNode）来提高下载稳定性。
- 对文件源无压力：一般只有少数几个 SuperNode 会从源下载文件。
- 支持标准 HTTP 头文件：支持通过 HTTP 头文件提交鉴权信息。
- 有效的 Registry 鉴权并发控制：减少对 Registry 鉴权服务的压力。
- 简单易用：仅需极少的配置。
 
### dragonfly原理
Dragonfly 下载普通文件和下载容器镜像的工作原理略有不同。

##### 下载普通文件
SuperNode 充当 CDN，并负责调度对等节点（Peer）之间的文件分块传输。dfget 是 P2P 客户端，也称为“Peer”（对等节点），主要用于下载和共享文件分块。

![image](https://d7y.io/docs/zh-cn/img/dfget.png)

#### 下载镜像文件
Registry 类似于文件服务器。dfget proxy 也称为 dfdaemon，会拦截来自 docker pull 或 docker push 的 HTTP 请求，然后使用 dfget 来处理那些跟镜像分层相关的请求。

![image](https://d7y.io/docs/zh-cn/img/dfget-combine-container.png)

#### 下载文件分块
每个文件会被分成多个分块，并在对等节点之间传输。一个对等节点就是一个 P2P 客户端。SuperNode 会判断本地是否存在对应的文件。如果不存在，则会将其从文件服务器下载到 SuperNode。

![image](https://d7y.io/docs/zh-cn/img/distributing.png)
 
### dragonfly部署（参考dragonfly官网）

https://d7y.io/zh-cn/

### dragonfly https的harbor 

dragonfly 常见问题

https://github.com/dragonflyoss/Dragonfly/blob/master/FAQ.md

目的：启用 docker PROXY  让dragonfly 支持https 
#### 1、部署https harbor

https://github.com/goharbor/harbor/blob/master/docs/configure_https.md

#### 2、部署docker_proxy
pull images

pull_images.sh
```
#!/bin/sh
docker_registry_proxy="dockerhubwp/docker_proxy_nginx:latest"
supernode="registry.cn-hangzhou.aliyuncs.com/alidragonfly/supernode:0.2.0"
dfclient="dockerhubwp/dfclient:latest"

images="${docker_registry_proxy} ${supernode} ${dfclient}"

function pullImage(){
   for image in ${images}; do
        echo -e "pull image ======>${image}"
        docker pull ${image}
    done
}

pullImage
```


docker_proxy.sh

```
#! /bin/sh

#   Separate deployment docker_proxy

# dfdaemon and docker registry map
# example  x.x.x
registry="harbor域名"
containername=docker_registry_proxy
#  你需要配置的dns 服务器 (如：dnsmasq)  
DNS_SERVER="dns-server"
docker_registry_proxy="dockerhubwp/docker_proxy_nginx:latest"

# get localhost ip
ipaddr=$(ip addr | awk '/^[0-9]+: / {}; /inet.*global/ {print gensub(/(.*)\/(.*)/, "\\1", "g", $2)}')
localhostIp=$(echo ${ipaddr} | cut -d " " -f 1)

function changeDockerProxy() {
    mkdir -p /etc/systemd/system/docker.service.d
	cat <<EOD >/etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:3128/"
Environment="HTTPS_PROXY=http://127.0.0.1:3128/"
EOD
}

function dockerDockerProxyRun() {
    if [[ 0 != $(docker ps -a | grep ${containername} | wc -l) ]]; then
        docker rm -f ${containername}
    fi
    docker run --restart=always --privileged=true --name ${containername} -d -p 0.0.0.0:3128:3128 -v /etc/docker_proxy_nginx/docker_mirror_certs:/ca -v /var/log/docker_proxy_nginx:/var/log/nginx/ -e DRAGONFLY_REGISTRIES="${registry},http://${localhostIp}:65001" -e REGISTRIES="${registry}" -e DNS_SERVER=${DNS_SERVER} ${docker_registry_proxy}
}

changeDockerProxy

systemctl daemon-reload
systemctl restart docker

dockerDockerProxyRun

```


#### 3、部署dragonfly

##### 部署Supernode

supernode.sh
```
#!/bin/sh

#   Separate deployment supernode

supernode="registry.cn-hangzhou.aliyuncs.com/alidragonfly/supernode:0.2.0"
containername=supernode

function superNode() {
    if [[ 0 != $(docker ps -a | grep ${containername} | wc -l) ]]; then
        docker rm -f ${containername}
    fi
    docker run --name ${containername} --restart=always -d -p 8001:8001 -p 8002:8002 ${supernode}
}
superNode
```
##### 部署dfclient
dfclient.sh

```
#!/bin/sh

#   Separate deployment docker_proxy

dfclient="dockerhubwp/dfclient:latest"

#harbor 地址
dfdaemon_registry="https://x.x.x"
containername=dfclient

# supernode ips  example (10.0.0.160,10.0.0.162)
supernodes="supernodeip"


ipaddr=$(ip addr | awk '/^[0-9]+: / {}; /inet.*global/ {print gensub(/(.*)\/(.*)/, "\\1", "g", $2)}')
localhostIp=$(echo ${ipaddr} | cut -d " " -f 1)

cat <<EOD >/etc/dragonfly.conf
[node]
address=${supernodes}
EOD


function startDfClient() {
    if [[ 0 != $(docker ps -a | grep ${containername} | wc -l) ]]; then
        docker rm -f ${containername}
    fi
    docker run --name ${containername} --restart=always -d  -p 65001:65001 -v /root/.small-dragonfly:/root/.small-dragonfly -v /etc/dragonfly.conf:/etc/dragonfly.conf  -e dfdaemon_registry=${dfdaemon_registry} -e localhostIp=${localhostIp} ${dfclient}
}

startDfClient
```

最后

trust.sh
```
#!/bin/sh

# trust ca

curl http://127.0.0.1:3128/ca.crt >/etc/pki/ca-trust/source/anchors/docker_proxy_nginx.crt

update-ca-trust
```


```
docker pull x.x.x/library/nginx:latest
```


参考：

   https://d7y.io/zh-cn/

   https://github.com/goharbor/harbor/
   
   https://github.com/rpardini/docker-registry-proxy
   
   https://github.com/chobits/ngx_http_proxy_connect_module
