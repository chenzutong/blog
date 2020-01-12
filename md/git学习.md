## 1 Git介绍
Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目.  
Git 不仅仅是个版本控制系统，它也是个内容管理系统(CMS)，工作管理系统等。  
Git 的内容存储使用的是 SHA-1 哈希算法。

## 2 Git安装
### Linux上安装
Git的工作需要调用curl,zlib,openssl,expat,libiconv等库的代码
#### Debian/Ubuntu
> apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev  
> apt-get install git

#### Centos/ReHat
> yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel  
> yum -y install git-core

#### Git配置
Git提供了一个叫git config的工具，专门用来配置或读取相应的工作环境变量。  
这些环境变量决定了Git在各个环节的具体工作方式和行为。  
这些变量可以存放在以下三个不同的地方：  
* /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。  
* ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。  
* 当前项目的 Git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：  
  这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。  
  
#### 用户信息
配置个人的用户名称和电子邮件地址  
> git config --global user.name "chenzutong"  
> git config --global user.email chenzutong912@163.com  

如果用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。  
如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。  

#### 文本编辑器
> git config --global core.editor emacs  

#### 差异分析工具
> git config --global merge.tool vimdiff  

#### 查看配置信息
> git config --list  

## Git工作流程
1. 克隆Git资源作为工作目录(git clone ...)
2. 在克隆的资源上添加或修改文件
3. 如果其他人修改Git资源，更新资源(git pull)
4. 在提交前查看修改(git status, git add .)
5. 提交修改(git commit -m "提交说明")
6. 推送操作(git push)

## Git工作区、暂缓区和版本库
* **工作区：** 电脑里能看到的目录
* **暂缓区：** 英文叫stage或index。一般存放在.git/index中
* **版本库：** 工作区中隐藏的.git目录

当对工作区修改（或新增）的文件执行 "git add" 命令时，暂存区的目录树被更新，
同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，
而该对象的ID被记录在暂存区的文件索引中。

当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，
master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。

当执行 "git reset HEAD" 命令时，暂存区的目录树会被重写，
被 master 分支指向的目录树所替换，但是工作区不受影响。

当执行 "git rm --cached <file>" 命令时，会直接从暂存区删除文件，
工作区则不做出改变。

当执行 "git checkout ." 或者 "git checkout -- <file>" 命令时，
会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，
会清除工作区中未添加到暂存区的改动。

当执行 "git checkout HEAD ." 或者 "git checkout HEAD <file>" 命令时，
会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。
这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

## Git创建仓库
### git init
Git使用git init命令来初始化一个Git仓库，Git的很多命令需要在Git的仓库中运行。  
执行完成git init命令后，Git仓库会生成一个.git目录，该目录包含了资源的所有元数据，其他的项目目录保持不变。  

#### 使用方法
使用当前目录作为Git仓库
> git init

使用指定目录作为Git仓库
> git init 指定目录

如果当前目录下有几个文件(如：test.py)想要纳入版本控制，
需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交.
> git add test.py
> git commit -m "初始化项目版本"

test.py提交到仓库中。

### git clone
> git clone <repo>

如果需要克隆到指定的目录：
> git clone <repo> <directory>

* **repo:** Git仓库
* **directory:** 本地目录

## Git基本操作
### 获取与创建项目命令
#### git init
创建新的 Git 仓库。

#### git clone
拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改

### 基本快照
#### git add
将该文件添加到缓存

#### git status
查看项目的当前状态

#### git diff
git diff 来查看执行 git status 的结果的详细信息。  
git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。  
* 尚未缓存的改动：git diff
* 查看已缓存的改动： git diff --cached
* 查看已缓存的与未缓存的所有改动：git diff HEAD
* 显示摘要而非整个 diff：git diff --stat

#### git commit
使用 git add 命令将想要快照的内容写入缓存区， 而执行 git commit 将缓存区内容添加到仓库中

如果你觉得 git add 提交缓存的流程太过繁琐，Git 也允许你用 -a 选项跳过这一步。
> git commit -a

#### git reset HEAD
取消已缓存的内容

#### git rm 

#### git mv
移动或重命名一个文件、目录、软连接。

## Git分支管理
### 列出分支
> git branch

### 创建分支
> git branch (branchname)

### 切换分支
> git checkout (branchanme)

### 新建并切换分支
> git checkout -b (branchname)

### 删除分支
> git branch -d (branchname)

### 分支合并
> git merge

### 合并冲突


## Git查看提交历史
列出历史提交记录
> git log


简洁
> git log --oneline


查看历史中什么时候出现了分支、合并
> git log --graph


--reverse逆向显示所有日志
> git log --reverse --oneline


--author指定用户
> git log --author=chenzutong --oneline -5


## Git标签
git tag

## Github
### Github简介
github是一个基于git的代码托管平台，
付费用户可以建私人仓库，我们一般的免费用户只能使用公共仓库，也就是代码要公开。

### 注册账号并创建仓库
https://github.com/

### 配置Git
创建ssh key
> ssh-keygen -t rsa -C "your_email@youremail.com"

成功的话会在~/下生成.ssh文件夹，进去，打开id_rsa.pub，复制里面的key.  
回到github上，进入 Account Settings（账户配置），
左边选择SSH Keys，Add SSH Key,title随便填，粘贴在你电脑上生成的key。


为了验证是否成功，在git bash下输入:
> ssh -T git@github.com


设置username和email,因为github每次commit都会记录他们
> git config user.name "your name"
> git config user.email "your_email@youremail.com"



进入要上传的仓库,添加远程地址
> git remote add origin git@github.com:yourName/yourRepo.git

