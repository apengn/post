---
title: redis
date: 2019-02-27 11:49:23
tags:
---

###  github.com/garyburd/redigo/redis  

#### 遇到的坑：

#### 并发
Connections支持Receive方法的一个并发调用者和Send和Flush方法的一个并发调用者。不支持其他并发，包括对Do和Close方法的并发调用。
要完全并发访问Redis，请使用线程安全池从goroutine中获取，使用和释放连接。从池返回的连接具有上一段中描述的并发限制。
#### 清空redis 数据
```
flushall
```