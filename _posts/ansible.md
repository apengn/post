---
title: ansible
date: 2019-04-12 23:44:42
tags:
---

#install ansible
```
python  --version
Python 2.7.10
sudo easy_install pip
sudo pip install ansible
```

dome :
https://blog.csdn.net/pushiqiang/article/details/78126063


```
ansible -i ./hosts local --connection=local -b --become-user=root -m shell -a "ls"
```

```
-b - “成为”，在运行命令时告诉可以成为另一个用户。
--become-user=root - 以用户“root”运行以下命令（例如，使用命令使用“sudo”）。我们可以在此定义任何现有的用户。
-a 用于将任何参数传递给定义的模块 -m
```

进行免密登陆
```
ssh-agent
ssh-copy-id root@127.0.0.1
```