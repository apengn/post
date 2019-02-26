---
title: docker 版kafka集群搭建
date: 2019-02-25 10:27:45
tags:
---

## kafka 集群搭建

参考:https://hub.docker.com/r/wurstmeister/kafka/tags

```
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka:
    image: wurstmeister/kafka
    ports:
      - "9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 10.0.0.159  #需要改成宿主ip
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock




```

```
docker-compose up -d
docker-compose scale kafka=3
```

