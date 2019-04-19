---
title: pprof
date: 2019-03-06 23:18:18
tags:
---


CPU Profiling：CPU 分析，按照一定的频率采集所监听的应用程序 CPU（含寄存器）的使用情况，可确定应用程序在主动消耗 CPU 周期时花费时间的位置
Memory Profiling：内存分析，在应用程序进行堆分配时记录堆栈跟踪，用于监视当前和历史内存使用情况，以及检查内存泄漏
Block Profiling：阻塞分析，记录 goroutine 阻塞等待同步（包括定时器通道）的位置
Mutex Profiling：互斥锁分析，报告互斥锁的竞争情况


cpu（CPU Profiling）: $HOST/debug/pprof/profile，默认进行 30s 的 CPU Profiling，得到一个分析用的 profile 文件
block（Block Profiling）：$HOST/debug/pprof/block，查看导致阻塞同步的堆栈跟踪
goroutine：$HOST/debug/pprof/goroutine，查看当前所有运行的 goroutines 堆栈跟踪
heap（Memory Profiling）: $HOST/debug/pprof/heap，查看活动对象的内存分配情况
mutex（Mutex Profiling）：$HOST/debug/pprof/mutex，查看导致互斥锁的竞争持有者的堆栈跟踪
threadcreate：$HOST/debug/pprof/threadcreate，查看创建新OS线程的堆栈跟踪


https://www.jianshu.com/p/4e4ff6be6af9

https://www.cnblogs.com/li-peng/p/9391543.html


go tool pprof httpdemo http://192.168.3.34:9909/debug/pprof/profile

top


list handleData

mac环境下graphviz安装及使用

https://blog.csdn.net/mouday/article/details/80902025
