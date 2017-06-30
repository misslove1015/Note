# Git常用命令

创建一个空目录

​$ mkdir git​​

​$ cd git

​​$ pwd

把这个目录初始化为Git可以管理的仓库

​$ git init

在目录下创建一个readme.rtf文件，把文件添加提交到仓库

​$ git add readme.rtf

​$ git commit -m “wrote a read file”

展示仓库当前的状态

​$ git status

文件被修改后，查看修改了什么内容

​$ git diff

查看提交的历史记录

​$ git log

缩略展示提交的历史记录

​$ git log- -pretty=oneline

回退到上一个版本

​$ git reset - -hard HEAD^

回退到上上个版本

​$ git reset - -hard HEAD^^

回退到前100个版本

​$ git reset- -hard HEAD~100

回退到某个版本

​$ git reset- -hard 123456（版本号，可以不写全）

找到输入过的命令

​$ git reflog

查看工作区和版本库里最新版本的区别

​$ git diff HEAD - - readme.rtf

丢弃工作区的修改

​$ git checkout - - readme.rtf

撤销暂存区的修改，放回工作区

​$ git reset HEAD readme.rtf

删除某个文件

​$ rm test.rtf

从版本库恢复删掉的文件

​$ git checkout - - test.rtf

从版本库删除某个文件

​$ git rm test.rtf

​​$ git commit -m “remove test.rtf”

创建SSH Key

​$ssh-keygen -t rsa -C“xxxxxxxx@163.com”

​id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人

把本地仓库的内容推送到GitHub的仓库

​$git remote add origin git@github.com:xxxxxxxxx/learngit.git

​$ git push -u origin master (第一次加-u参数)

把本地仓库提交到服务器

​$ git push origin master

把服务器的仓库克隆到本地

​$git clone git@github.com:xxxxxxxxx/gitskills.git

​$ git clonehttps://github.com/xxxxxxxxx/gitskills.git

创建一个dev分支，并切换到dev分支

​$ git checkout -b dev

-b表示创建并切换，等价于

​$ git branch dev

​$ git checkout dev

查看当前分支

​$ git branch

把dev分支合并到当前分支

​$ git merge dev

删除dev分支

​$ git branch -d dev

查看分支合并情况

​$git log - -graph - -pretty=oneline - -abbrev-commit

以普通模式合并，保留分支的信息

​$git merge --no-ff -m"merge with no-ff"dev

保存当前工作现场，以后可以恢复继续工作

​$ git stash

显示存储的工作现场

​$ git stash list

恢复工作现场，并且不删除存储的工作现场

​$ git stash apply

删除存储的工作现场

​$ git stash drop

恢复工作现场，并删掉存储的工作现场

​$ git stash pop

强行删除一个没有被合并的分支

​$git branch -D

查看远程仓库的信息

​$ git remote

查看远程仓库更详细的信息

​$  git remote -v

推送到服务器的dev分支

​$ git push origin dev

获取服务器最新更改

​$ git pull

在本地创建和远程分支对应的分支

​$git checkout -b branch-name origin/branch-name

创建本地分支和远程分支的链接关系

​$ git branch --set-upstream branch-name origin/branch-name

在Git中打一个标签，首先切换到需要打标签的分支上

​$ git tag v1.0

在对应的历史提交上打标签

​$ git tag v0.9 123456(提交的id)

查看标签信息

​$ git show v0.9

指定标签信息

​$ git tag -a -m “abcd…”

用PGP签名标签

​$ git tag -s -m “abcd…”

删除标签

​$ git tag -d v0.1

推送标签到服务器

​$ git push origin v1.0

推送所有未推送的标签到服务器

​$ git push origin - - tags

删除远程的标签，先删除本地，然后从远程删除

​$ git push origin :refs/tags/v0.9

忽略仓库里某些文件，让其不参与提交，可编写.gitignore文件，然后输入要忽略的文件名

配置别名，比如st表示status，co表示checkout，ci表示commit，br表示branch

​$ git config - -globalalias.ststatus

​$ git config - -globalalias.cocheckout

​$ git config - -globalalias.cicommit

​$ git config - -globalalias.brbranch

搭建Git服务器

首先准备一台运行Linux的机器

​​1.安装git

​$ sudoapt-getinstall git

​2.创建一个git用户

​​​​$ sudoadduser git

​​​3.创建证书登录

​​​手机用户的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个

​​​4.初始化Git仓库

​​先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

​​$ sudogit init --bare sample.git

​​Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

​​$ sudochown -R git:git sample.git

​5.禁用shell登录

​出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

​git:x:1001:1001:,,,:/home/git:/bin/bash

​改为：

​git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

​这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出

​6.克隆远程仓库

​​$git clone git@server:/srv/sample.git