---
title: 容器化日志收集方案-EFK
date: 2019-02-26 12:12:53
tags: [log]
---
本文将描述如何使用
elasticsearch、
fluentd 、
kibana构建容器化日志收集系统

### 介绍：

###### elasticsearch：

- 一个分布式的实时文档存储，每个字段 可以被索引与搜索

- 一个分布式实时分析搜索引擎

- 能胜任上百个服务节点的扩展，并支持 PB 级别的结构化或者非结构化数据

  

###### fluentd:
是日志收集系统，通过丰富的插件，可以收集来自于各种系统或应用的日志，然后根据用户定义将日志做分类处理。 

###### kibana:
是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据。 


###### Kafka:
是分布式的、可分区的、可复制的消息系统。它提供了普通消息系统的功能，但具有自己独特的设计

Kafka将消息以topic为单位进行归纳。
将向Kafka topic发布消息的程序成为producers.
将预订topics并消费消息的程序成为consumer.


## 如何安装
- ##### 安装kafka+zookeeper

参考：https://hub.docker.com/r/wurstmeister/kafka/

-  ##### 安装elasticsearch+kibana:

```cmd
docker run -d -v "$PWD/esdata":/usr/share/elasticsearch/data -p 9200:9200  elasticsearch
docker run --name kibana -e ELASTICSEARCH_URL=http://127.0.0.1:9200 -p 5601:5601 -d kibana
```

- ##### 安装fluentd

###### 1、自定义fluentd的Dockerfile

```
FROM fluent/fluentd:v1.2

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish

RUN apk add --update --virtual .build-deps \
                                sudo \
                                build-base \
                                ruby-dev \
                                tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && sudo gem install fluent-plugin-elasticsearch \
    && sudo gem install fluent-plugin-kafka \
    && sudo gem sources --clear-all \
    && apk del .build-deps \
    && rm -rf /var/cache/apk/* \
              /home/fluent/.gem/ruby/2.3.0/cache/*.gem

ENTRYPOINT ["fluentd", "-c", "/fluentd/etc/fluent.conf", "-p", "/fluentd/plugins"]
```

###### 2、编写fluent.conf

```
<system>
  rpc_endpoint 127.0.0.1:24444
</system>
<source>
	@type forward
	@id forward_input
</source>

<source>
		@type    tail  #### tail方式采集日志
		#format   none
		format   /^(?<all>.*)$/
		path     /log/log.txt
		pos_file /log/pos_file/httpd-access.log.pos
		tag      log.tag
</source>

<match log.**>
       @type copy
       <store>
        @type elasticsearch
        hosts localhost:9200
        type_name elasticsearch_fluentd
        include_tag_key true
        tag_key log_name
        flush_interval 10s # for testing
        logstash_format true
        logstash_prefix logstash
       </store>
       <store>
         @type     kafka
		brokers              localhost:9092
		#zookeeper           localhost:2181
		default_topic       fluent_kafka
		#刷新间隔
		flush_interval      30
		ack_timeout      2000
		output_data_type    attr:all
       </store>

</match>

<source>
	@type monitor_agent
	@id monitor_agent_input
	port 24220
</source>

<match debug.**>
	@type stdout
</match>


```
###### 3、构建fluentd image:

```
docker build -t fluentd:test -f Dockerfile .
```

###### 4、启动fluentd

```
docker run -it --rm --name fluent_test -v $(pwd)/log/:/log/ -v $(pwd)/:/fluentd/etc/ fluentd:test
```

==说明==

配置fluentd的RPC 为了重新加载fluentd.conf文件
参考：https://docs.fluentd.org/v1.0/articles/rpc

```
<system>
  rpc_endpoint 127.0.0.1:24444
</system>
```

### 测试

```
增加这个文件的日志    echo  test > $(pwd)/log/log.txt
```

在kibana中进行日志查看：

![image](https://note.youdao.com/yws/api/personal/file/FB957D2F60304CDC9043EFF343CB6B61?method=download&shareKey=a31b0bd854f7e7fad8dd9f952117006f)

修改fluent.conf

```
tag_key log_name 修改为tag_key log_name_key
执行curl http://127.0.0.1:24444/api/config.reload
```

即可以重新加载fluent.conf配置文件，使其生效
继续向log.txt写入日志





fluentd向kafka写入日志

进入kafka的container中

执行

```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic fluent_kafka --from-beginning 
```



附上kafka基本命令 

```
创建topic 这里创建的topic为：
bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic 

查看topic
king bin/kafka-topics.sh --list --zookeeper zookeeper:2181

挂起生产者：producer
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

挂起之后，就可以输入信息：test
挂起消费者： consumer:
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

在consulmer就可以看到生产出来的：test
```



至此完成了基本的elk  和fluent+kafka+zookeeper的日志系统

在kubernets中使用DaemonSet  来获取主机日志或docker container日志



参考：https://docs.fluentd.org/v1.0/articles/quickstart

​           https://www.elastic.co/products





   