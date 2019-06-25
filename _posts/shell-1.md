---
title: shell基本命令
date: 2019-04-21 09:36:41
tags:
---
## shell基本命令
把多个文件夹中的内容复制到同一个文件下
```$xslt
cp -r ./file1 ./file2  /root
```
uname -r 获取系统版本
```$xslt
sh-4.2# uname -r
3.10.0-957.10.1.el7.x86_64
```
unset重新设置变量内容
```$xslt
unset name
```
date 格式化
```$xslt
sh-4.2# date +%Y/%m/%d
2019/04/21

sh-4.2# date +%H:%m
08:04
```
cal 查看日历

bc 计算器

搜索：
```$xslt
/string  向上搜索N
?string  向下搜索n
```
正确的关机方法
```bash
shutdown
poweroff
halt 
```
重启方法
```
reboot
```
添加用户和密码
```
useradd wp
passwd wp
```