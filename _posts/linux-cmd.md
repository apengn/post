---
title: linux_cmd
date: 2019-02-26 22:02:28
tags:
---

ps -ef |grep {fillter}
kill -9 {pid}
mkdir -p roles/{common,install}/{handlers,files,meta,tasks,templates,vars
ss -tunlp|grep  7777
ss -alnp |grep 7777
netstat -tunlp|grep 777
netstat -alnp|grep 7777
lsof -i tcp:777
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

kcc get pod |grep orchestration | awk '{print $1}'


screen 使用
screen 
执行逻辑

screen -list 

screen -r 



screen -r 30719.pts-4.dev-10
There is a screen on:
	30719.pts-4.dev-10	(Attached)
There is no screen to be resumed matching 30719.pts-4.dev-10.
[root@dev-10 ~]# screen -r 30719.pts-4.dev-


netstat -an|wc -l   wc 统计行号


kubectl get -o template node/dev-9 --template={{.spec}}

linux 部分快捷键
1. Tab
这是你不能没有的 Linux 快捷键。它将节省你 Linux 命令行中的大量时间。

只需要输入一个命令，文件名，目录名甚至是命令选项的开头，并敲击 tab 键。 它将自动完成你输入的内容，或为你显示全部可能的结果。

如果你只记一个快捷键，这将是必选的一个。

2. Ctrl + C
这些是为了在终端上中断命令或进程该按的键。它将立刻终止运行的程序。

如果你想要停止使用一个正在后台运行的程序，只需按下这对组合键。

3. Ctrl + Z
该快捷键将正在运行的程序送到后台。 通常，你可以在使用 & 选项运行程序前之完成该操作， 但是如果你忘记使用选项运行程序，就使用这对组合键。

4. Ctrl + D
这对键盘快捷键将使你退出当前终端。如果你使用 SSH 连接，它将会关闭。 如果你直接使用一个终端，该应用将会立刻关闭。

把它当成“退出”命令。

5. Ctrl + L
你怎么清空你的终端屏幕？我猜是用 clear 命令。

你可以使用 Ctrl+L 清空终端，代替输入 C-L-E-A-R。得心应手，不是吗？

6. Ctrl + A
该快捷键将移动光标到所在行首。

假设你在终端输入了一个很长的命令或路径，并且你想要回到它的开头， 使用方向键移动光标将花费大量时间。注意你无法使用鼠标移动光标到行首。

这是 Ctrl+A 节省时间的地方。

7. Ctrl + E
这对快捷键与 Ctrl+A 相反。 Ctrl+A 送光标到行首，反之 Ctrl+E 移动光标到行尾。

8. Ctrl + U
输入了错误的命令？ 代替用退格键来丢弃当前命令，使用 Linux 终端中的 Ctrl+U 快捷键。 该快捷键会擦除从当前光标位置到行首的全部内容。

9. Ctrl + K
这对和 Ctrl+U 快捷键有点像。 唯一的不同在于不是行首，它擦除的是从当前光标位置到行尾的全部内容。

10. Ctrl + W
你刚才了解了擦除到行首和行尾的文本。 但如果你只需要删除一个单词呢？使用 Ctrl+W 快捷键。

使用 Ctrl+W 快捷键，你可以擦除光标位置前的单词。 如果光标在一个单词本身上，它将擦除从光标位置到词首的全部字母。

最好的方法是用它移动光标到要删除单词后的一个空格上， 然后使用 Ctrl+W 键盘快捷键。

11. Ctrl + Y
这将粘贴使用 Ctrl+W，Ctrl+U 和 Ctrl+K 快捷键擦除的文本。 如果你删除了错误的文本或需要在某处使用已擦除的文本，这将派上用场。

12. Ctrl + P
你可以使用该快捷键来查看上一个命令。 你可以反复按该键来返回到历史命令。 在很多终端里，使用 PgUp 键来实现相同的功能。

13. Ctrl + N
你可以结合 Ctrl+P 使用该快捷键。Ctrl+N 显示下一个命令。 如果使用 Ctrl+P 查看上一条命令，你可以使用 Ctrl+N 来回导航。 许多终端都把此快捷键映射到 PgDn 键。

14. Ctrl + R
你可以使用该快捷键来搜索历史命令。

```
