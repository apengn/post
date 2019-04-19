---
title: yum  源加速设置
date: 2019-04-13 10:29:37
tags:
---

yum 安装完成后生成的配置文件及目录：

主配置文件：/etc/yum.conf
资源库配置目录：/etc/yum.repos.d
重要文件： /etc/yum.repos.d/CentOS-Base.repo
yum 加速插件：
实现的功能：可以自动选择速度最快的镜像

安装yum 加速插件： 
yum install yum-plugin-fastestmirror
加速插件的配置文件：
/etc/yum/pluginconf.d/fastestmirror.conf
yum镜像的速度测试记录文件：
/var/cache/yum/timedhosts.txt
更换系统默认yum 源
举例：以更换yum源为阿里云yum源

备份系统默认的yum源
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

下载阿里云yum源

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

添加 epel 源

yum -y install epel-release.noarch
常用的yum源
epel源：https://fedoraproject.org/wiki/EPEL
repoforge源：http://repoforge.org/use/
php和mysql源：https://webtatic.com

清理缓存
yum clean all

生成新的缓存
yum makecache