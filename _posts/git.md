---
title: git
date: 2019-02-26 22:02:36
tags:
---

Git

新建分支：v1.1.1

```
git branch v1.1.1

```
或

```
git branch -b v1.1.1
```


//新建分支并切换到新建分支
home@DESKTOP-S967PDA MINGW64 /f/gitPractise/gitDemo (v1.1.2)

```
$ git checkout -b v1.1.5 v1.1.2
```



切换分支：

```
git checkout v1.1.1
```


//把分支push 到remote


```
git push orign v1.1.1
```



//修改branch 的名字


```
git branch -m v1.1.1 v1.1.2
```


//删除branch


```
git push origin --delete v1.1.1
```





添加文件到git
将当前目录下所有变化的文件，放入暂存区


```
git add .
git add ./
```


//参数表示只添加暂存区已有的文件（包括删除操作），但不添加新增的文件。



```
git add -u
```





//查看本地branch 分支


```
git branch 

git branch -v
```


//查看本地和远程的分支


```
git branch -a
```


//查看 远程remote 分支

```
git branch -r
```



//比较文件差异


```
git diff
```


//删除分支(前提条件是该分支没有被合并)


```
git branch -d v1.1.1
```

//强制删除一个分支，不管该分支有没有被合并

git branch -D v1.1.1


//查看merge的情况
git branch --merged

将工作区制定的文件还原到上次commit的状态
git checkout xxx.txt

//切换到某个tag

git checkout tags/1.1.4
或
git checkout 1.1.4


//命令”复制”一个提交节点并在当前分支做一次完全一样的新提交。



```
git cherry-pick xxxxx
```

//根据一个树对象，生成新的commit对象。

git commit-tree 16e19f -m “First commit”


查看工作区与暂存区的差异


```
$ git diff
```

 查看某个文件的工作区与暂存区的差异

```
$ git diff file.txt
```

查看暂存区与当前 commit 的差异

```
$ git diff --cached
```

```

```

查看两个commit的差异

```
$ git diff <commitBefore> <commitAfter
```
>
 查看暂存区与仓库区的差异

```
$ git diff --cached
```

查看工作区与上一次commit之间的差异
即如果执行 git commit -a，将提交的文件

```
$ git diff HEAD
```

查看工作区与某个 commit 的差异

```
$ git diff <commit>
```

显示两次提交之间的差异

```
$ git diff [first-branch]...[second-branch]
```

查看工作区与当前分支上一次提交的差异，但是局限于test文件

```
$ git diff HEAD -- ./test
```

查看当前分支上一次提交与上上一次提交之间的差异

```
$ git diff HEAD -- ./test
```

生成patch

```
$ git format-patch master --stdout > mypatch.patch
```

```

```



 查看topic分支与master分支最新提交之间的差异

```
$ git diff topic master
```

 与上一条命令相同

```
$ git diff topic..master
```

查看自从topic分支建立以后，master分支发生的变化

```
$ git diff topic...master
```

```

```



命令将当前目录转为git仓库。


```
git init
```




命令按照提交时间从最晚到最早的顺序，列出所有 commit。

```
git log
```

git remote
为远程仓库添加别名。


```
it remote add john git@github.com:johnsomeone/someproject.git
```
 显示所有的远程主机

```
git remote -v
```

 列出某个主机的详细信息

```
git remote show name
```
git revert
git revert命令用于撤销commit。


```
git revert <commitID>
```

git rm
git rm命令用于删除文件。

解除追踪某个文件，即该文件已被git add添加，然后抵消这个操作。

```
$ git rm --cached <fileName>



git show
git show命令用于查看commit的内容

# 输出某次提交的元数据和内容变化
$ git show [commit]
$ git show 12a86bc38 # By revision
$ git show v1.0.1 # By tag
$ git show feature132 # By branch name
$ git show 12a86bc38^ # Parent of a commit
$ git show 12a86bc38~2 # Grandparent of a commit
$ git show feature132@{yesterday} # Time relative
$ git show feature132@{2.hours.ago} # Time relative

```




git merge master
```
home@DESKTOP-S967PDA MINGW64 /f/gitPractise/gitDemo (v1.1.5)
$ git merge master
Already up-to-date.

home@DESKTOP-S967PDA MINGW64 /f/gitPractise/gitDemo (v1.1.5)
$ git checkout v1.1.5
Already on 'v1.1.5'

home@DESKTOP-S967PDA MINGW64 /f/gitPractise/gitDemo (v1.1.5)
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.

home@DESKTOP-S967PDA MINGW64 /f/gitPractise/gitDemo (master)
$ git merge v1.1.5
Updating 891e496..f6a3810
Fast-forward
 dome.txt | 1 +
 1 file changed, 1 insertion(+)

home@DESKTOP-S967PDA MINGW64 /f/gitPractise/gitDemo (master)
$ cat dome.txt
gjkdsjsdjfjjgjksjdjfkdsjfsjffjkdsjfsjkfjksjfjsdjjsf
```


查看提交日志

```
git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
```
回退版本


```
git reset --hard 1094adb7b9b3807259d8cb349e7df1d4d6477073
```




查看远程分支
加上-a参数可以查看远程分支，远程分支会用红色表示出来（如果你开了颜色支持的话）：
$ git branch -a
  master
  remote
  tungway
  v1.52
* zrong
  remotes/origin/master
  remotes/origin/tungway
  remotes/origin/v1.52
  remotes/origin/zrong
删除远程分支和tag
在Git v1.7.0 之后，可以使用这种语法删除远程分支：

1
$ git push origin --delete <branchName>
删除tag这么用：

1
git push origin --delete tag <tagname>
否则，可以使用这种语法，推送一个空分支到远程分支，其实就相当于删除远程分支：

1
git push origin :<branchName>
这是删除tag的方法，推送一个空tag到远程tag：

$ git remote show origin
* remote origin
  Fetch URL: git@github.com:xxx/xxx.git
  Push  URL: git@github.com:xxx/xxx.git
  HEAD branch: master
  Remote branches:
    master                 tracked
    refs/remotes/origin/b1 stale (use 'git remote prune' to remove)
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
这时候能够看到b1是stale的，使用 git remote prune origin 可以将其从本地版本库中去除。

更简单的方法是使用这个命令，它在fetch之后删除掉没有与远程分支对应的本地分支：

1
git fetch -p
重命名远程分支
在git中重命名远程分支，其实就是先删除远程分支，然后重命名本地分支，再重新提交一个远程分支。

例如下面的例子中，我需要把 devel 分支重命名为 develop 分支：

$ git branch -av
* devel                             752bb84 Merge pull request #158 from Gwill/devel
  master                            53b27b8 Merge pull request #138 from tdlrobin/master
  zrong                             2ae98d8 modify CCFileUtils, export getFileData
  remotes/origin/HEAD               -> origin/master
  remotes/origin/add_build_script   d4a8c4f Merge branch 'master' into add_build_script
  remotes/origin/devel              752bb84 Merge pull request #158 from Gwill/devel
  remotes/origin/devel_qt51         62208f1 update .gitignore
  remotes/origin/master             53b27b8 Merge pull request #138 from tdlrobin/master
  remotes/origin/zrong              2ae98d8 modify CCFileUtils, export getFileData
删除远程分支：


```
$ git push --delete origin devel
```

To git@github.com:zrong/quick-cocos2d-x.git
 - [deleted]         devel
重命名本地分支：

1

```
git branch -m devel develop
```

推送本地分支：

$ git push origin develop
Counting objects: 92, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (48/48), done.
Writing objects: 100% (58/58), 1.38 MiB, done.
Total 58 (delta 34), reused 12 (delta 5)
To git@github.com:zrong/quick-cocos2d-x.git
 * [new branch]      develop -> develop
然而，在 github 上操作的时候，我在删除远程分支时碰到这个错误：


$ git push --delete origin devel
remote: error: refusing to delete the current branch: refs/heads/devel
To git@github.com:zrong/quick-cocos2d-x.git
 ! [remote rejected] devel (deletion of the current branch prohibited)
error: failed to push some refs to 'git@github.com:zrong/quick-cocos2d-x.git'
这是由于在 github 中，devel 是项目的默认分支。要解决此问题，这样操作：

进入 github 中该项目的 Settings 页面；
设置 Default Branch 为其他的分支（例如 master）；
重新执行删除远程分支命令。
把本地tag推送到远程
1
git push --tags
获取远程tag
1
git fetch origin tag <tagname>


保存当前修改在缓存中，不可见

```
git stash save "worker"
```
把当前保存在缓存的显示出来，可见

```
 git stash pop

```

## Git-命令行-使用 Tag 标记你的代码 ## 
参考
https://blog.csdn.net/qq_32452623/article/details/73949509

https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001376951885068a0ac7d81c3a64912b35a59b58a1d926b000

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

同步remote branch to local 
```
git push -p
```


## 对单次提交的每个文件都添加注释
可以单独add/commit，然后最后一起push。

如有两个文件A.md和B.md需要push， 可以如下操作：


```
git add A.md
git commit -m "add A"
git add B.md
git commit -m "add B"
git push.....
```


git squash

`Git进阶命令讲解：squash,fixup,stash
2014-05-21 GitHub不完全装B指南

@董林 希望我讲一下这三个命令，我看了一下，这三个命令虽然平时用得较少，但是在特定的情况下还是非常有用的，所以我就详细讲解一下。



注意：下面的内容可能有点难，大家要集中精力了。



一、squash



squash准确来说并不是一个命令，而是rebase命令的一个功能。squash的作用很简单——合并多个commit。



来看用法：

git rebase -i HEAD~5
-i的意思是使用“交互式”的修改方法。加了这个参数之后，Git会把所有commit列出来，让你进行一些修改，修改完成之后会根据你的修改来rebase。HEAD-5的意思是只修改最近的5个commit。



运行完这条命令之后，会进入一个编辑界面，大概是这样的：

pick awe Add sth
pick add Have a rest
pick xxc Wow
pick cxz Yeah
pick dsa Finish it
可以看到一共有5行，这就是最近的5个commit以及它们的信息。



假设我们现在想把它们合并成一个commit，要怎么做呢？直接看例子：

pick awe Add sth
squash add Have a rest
squash xxc Wow
squash cxz Yeah
squash dsa Finish it



我们把后4个commit的pick都改成了squash，修改完成之后保存退出，Git就会继续执行rebase命令了。执行完可以看一下结果，最后的4条commit都消失了，它们都被合并到了倒数第5条中。



现在大家应该理解了，squash的作用就是把当前commit向前合并，一直合并到pick为止。



rebase完成之后大家可以看一下最后一个commit的信息，里面会包含rebase之前最后5条的所有commit信息，也就是诸如“Add sth Have a rest Wow Yeah Finish it”这些东西。



二、fixup



fixup和squash非常类似，把pick修改成fixup，同样会向前合并到pick，唯一的区别就是，fixup会忽略当前commit的信息，只会应用修改。



怎么理解呢？还用上面的例子说，假设我们使用fixup代替squash：

pick awe Add sth
fixup add Have a rest
fixup xxc Wow
fixup cxz Yeah
fixup dsa Finish it



保存退出之后，等Git执行完rebase我们查看一下最后一条commit的信息，会发现里面只有“Add sth”，另外四条commit的信息比如“Have a rest”都没有了。



三、stash



stash是一条Git命令，作用非常简单——保存当前状态。



假设我们现在正在进行工作，修改了一些文件，添加了一些文件。这时我们突然想切换到另一个分支工作，但是又不想commit当前的修改（可能因为它们还不能运行），那该怎么办呢？答案就是stash。



来看命令——git stash



执行完命令之后，Git会把当前的状态保存，同时清理当前目录。我们可以运行一下 git status，会发现当前目录没有任何修改。这时我们就可以放心地切换分支工作了。



等其他工作结束，想继续之前的工作时，可以运行 git stash apply，Git会把保存的状态复原。



stash可以运行多次，会保存多个状态，可以运行 git stash list 来查看所有状态。运行 git stash apply 会默认复原到最近一次保存的状态，如果想指定复原状态可以使用 git stash apply stash@{2} ，这条命令会复原到 stash@{0} 状态。



运行 apply 之后，被复原的状态并不会自动删除，仍然在 stash list 当中。可以运行 git stash drop stash@{0} 来删除 stash@{0} 状态，或者使用 git stash pop 命令来代替 git stash apply 命令，这样复原之后会自动删除被复原状态。



如果 apply 的时候出现冲突（因为你 stash 之后对文件进行了修改），那么需要手动解决冲突。

最后需要注意一点：stash 复原的时候默认不复原 staged 文件，也就是说如果你运行过 add 命令，使用 git status 查看的话文件应当处于 staged 状态，但是如果你 stash 并复原再看，那个文件的状态又变回 unstaged 了。解决办法就是运行 git stash apply 命令时加上 --index 参数：git stash apply --index，这样就可以完全恢复到 stash 之前的状态。``

```



修改branch 的名字
```
git branch -M oldbranch newbranch
```


git stash 可用来暂存当前正在进行的工作， 比如想pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。
基础命令：
$git stash
$do some work
$git stash pop


进阶：

git stash save "work in progress for foo feature"

当你多次使用’git stash’命令后，你的栈里将充满了未提交的代码，这时候你会对将哪个版本应用回来有些困惑，

’git stash list’ 命令可以将当前的Git栈信息打印出来，你只需要将找到对应的版本号，例如使用’git stash apply stash@{1}’就可以将你指定版本号为stash@{1}的工作取出来，当你将所有的栈都应用回来的时候，可以使用’git stash clear’来将栈清空。



git stash          # save uncommitted changes
# pull, edit, etc.
git stash list     # list stashed changes in this git
git show stash@{0} # see the last stash 
git stash pop      # apply last stash and remove it from the list

git stash --help   # for more info

git checkout bbb
git rebase master   //   把master的代码弄到bbb上(前提条件是文件不冲突，tree进行改变)

git checkout --theirs xxx    把xxx  分支的内容放到bbb上

git checkout --ours xxx  类似


git删除某个commit

```
1.git log 获取commit信息 
2.git rebase -i (commit-id) 
commit-id 为要删除的commit的下一个commit号 
3.编辑文件，将要删除的commit之前的单词改为drop 
4.保存文件退出大功告成 
5.git log查看

git push origin master –force
```