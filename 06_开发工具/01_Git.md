<h1 id="Git" align="center">Git</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. Git 基础](#1-git-基础)
  - [1.1. 设置全局配置信息](#11-设置全局配置信息)
  - [1.2. 使用 Git 仓库](#12-使用-git-仓库)
  - [1.3. 版本回退](#13-版本回退)
  - [1.4. 工作区和暂存区](#14-工作区和暂存区)
  - [1.5. 分支](#15-分支)
  - [1.6. 冲突](#16-冲突)
  - [1.7. 存储工作现场](#17-存储工作现场)
  - [1.8. 推送和抓取远程分支](#18-推送和抓取远程分支)
  - [1.9. 标签](#19-标签)
  - [1.10. gitignore](#110-gitignore)
- [2. 配置 GitHub 的 SSH 密钥](#2-配置-github-的-ssh-密钥)
- [3. Git常用命令](#3-git常用命令)
- [4. Git Commit message 编写指南](#4-git-commit-message-编写指南)
  - [4.1. Header 包括三部分](#41-header-包括三部分)

<!-- /code_chunk_output -->

# 1. Git 基础

Git 是 C 语言开发的分布式的版本控制系统，而 SVN 是集中式的版本控制系统。Git 的客户端不只是提取最新版本的快照，而且将整个代码仓库镜像复制下来。

**为什么要使用Git**
- 能够对文件版本控制和多人协作开发
- 拥有强大的分支特性，所以能够灵活地以不同的工作流协同开发
- 分布式版本控制系统，即使协作服务器宕机，也能继续提交代码或文件到本地仓库，当协作服务器恢复正常工作时，再将本地仓库同步到远程仓库。
- 当团队中某个成员完成某个功能时，通过pull request操作来通知其他团队成员，其他团队成员能够review code后再合并代码。
> 作者：Sam_Lau
> 链接：https://www.jianshu.com/p/67afe711c731



## 1.1. 设置全局配置信息

```shell
# 设置设置全局用户名和邮箱
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
# 查看全局配置信息
git config --global --list
```

## 1.2. 使用 Git 仓库

初始化一个Git仓库，使用 `git init` 命令。

添加文件到Git仓库，分两步：  
使用命令 `git add <file>` ，注意，可反复多次使用，添加多个文件；  
使用命令 `git commit -m <message>` ，完成。

要随时掌握工作区的状态，使用 `git status` 命令。

如果git status告诉你有文件被修改过，用 `git diff` 可以查看修改内容。

## 1.3. 版本回退

HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 `git reset --hard [commit_id]` （上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100）。

穿梭前，用 `git log` 可以查看提交历史，以便确定要回退到哪个版本。
`git log --oneline` 每条 log 显示在一行内
`git log --graph `  树形显示

要重返未来，用 `git reflog` 查看命令历史，以便确定要回到未来的哪个版本。

## 1.4. 工作区和暂存区

工作区：电脑里能看到的目录，当前文件资源管理器直接看到的目录。  
版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；  
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

```shell
# 将工作区中修改后的 readme.txt 恢复到 暂存区版本（工作区中的 readme.txt 内容变为暂存区中的内容 ）
git resotre readme.txt

# 将暂存区中修改后的 readme.txt 恢复到 本地仓库版本 ，回滚 git add (不影响工作区中的 readme.txt 的内容)
git restore --staged readme.txt
```

## 1.5. 分支

查看分支：`git branch` `git branch -a`

创建分支：`git branch <name>`

切换分支：`git checkout <name>` 或者 `git switch <name>`

创建+切换分支：`git checkout -b <name>` 或者 `git switch -c <name>`

合并某分支到当前分支：`git merge <name>`  
`git merge --no-ff -m "<合并消息>" <name>` --no-ff 会保留分支历史

删除分支：`git branch -d <name>`  
如果要丢弃一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除。

## 1.6. 冲突

回到解决冲突之前的状态：`git merge --abort`

## 1.7. 存储工作现场

存储被Git管理的文件：`git stash`，再用 git status 查看就是干净的，注意没有被 git 追踪的文件不会被存储

查看存储的现场：`git stash list`

用 `git stash apply` 恢复，再用 `git stash drop` 来删除；

用 `git stash pop`，恢复的同时把stash内容也删了：

## 1.8. 推送和抓取远程分支

查看远程库信息：`git remote -v`

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支：`git push origin <branch-name>`，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支：`git checkout -b <branch-name> origin/<branch-name>`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联： `git branch --set-upstream <branch-name> origin/<branch-name>`；

从远程拉取分支：`git pull`

## 1.9. 标签

给本次提交添加 tag：`git tag v1.0`  
给指定的提交添加 tag：`git tag v1.0 <commit id>`  
创建带有说明的标签，用-a指定标签名，-m指定说明文字：`git tag -a v0.1 -m "version 0.1 released" 1094adb`

查看标签：`git tag`

推送某个标签到远程：`git push origin <tagname>`  
一次性推送全部尚未推送到远程的本地标签：`git push origin --tags`

删除本地标签：`git tag -d v0.9`   
删除远程标签：`git push origin :refs/tags/v0.9`

## 1.10. gitignore

检查.gitignore文件书写是否正确：`git check-ignore`  
`git check-ignore -v App.class`

# 2. 配置 GitHub 的 SSH 密钥

生成一个SSH keys

```
ssh-keygen -t rsa -C "lengjsong@outlook.com"
```

然后会出现:

```
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/aBadString/.ssh/id_rsa):
```

就是让你输入SSH keys要保存在哪里,一般不用改,就直接回车就好了.

```
Created directory '/c/Users/aBadString/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

输入两次密码之后，就会生成 key（不想输入密码直接回车即可）

```
Your identification has been saved in /c/Users/aBadString/.ssh/id_rsa.
Your public key has been saved in /c/Users/aBadString/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:+GnYA2zlkbeoLLpkhAvBYiawGD58DRXNciTRMnwUMws lengjsong@outlook.com
The key's randomart image is:
+---[RSA 2048]----+
|o  .oEO*.        |
|*o  o=o*+.       |
|*B . .*.+ .      |
|=oo  . + + .     |
|o .   = S .      |
|.o   o * .       |
|. o . + *        |
| o . . . .       |
|  o.             |
+----[SHA256]-----+
```

然后来到 GitHub 网站。点击一个储存库的 setting ---> Deploy keys ---> Add deploy keys：

![image-20200312224657261](/images/tool/image-20200312224657261.png)

测试连接，回到 git bash 执行

```
ssh -T git@github.com
```

回答 yes，并出现下面信息则表示链接上了。

![image-20200312225017231](/images/tool/image-20200312225017231.png)

设置一下全局的 username 和 email

```
git config --global user.name "aBadString"
git config --global user.email "lengjsong@outlook.com"
```

创建一个本地仓库

```
mkdir aBadString.github.io
cd aBadString.github.io
```

```
git init
```

创建一个 README.md 文件

```
echo "# aBadString.github.io" >> README.md
```

将 README.md 文件添加到暂存库

```
git add README.md
```

提交

```
git commit -m "first commit"
```

将本地仓库关联远程GitHub仓库地址

```
git remote add origin git@github.com:aBadString/aBadString.github.io.git
```

推送所以本地提交到远程仓库

```
git push -u origin master
```

浏览器访问地址 ```https://abadstring.github.io/``


# 3. Git常用命令

```bash
# 配置并查看全局信息
git config --global user.name 'aBadString'
git config --global user.email ''
git config -l

# 初始化
git init

# 连接远程库
git remote add origin http...
# 查看本地变化
git status

# 提交
git add *
git commit -m 'first_commit'
git push -u origin master

# 删除暂存区的文件，不删除工作区的文件
git rm -r --cached 文件名
# 撤销上一次 git add
git reset HEAD
# 撤销上一次 git commit, 但不删除文件
git reset --soft HEAD^

# 可以通过如下命令进行代码合并【注：pull=fetch+merge]
git pull --rebase origin master

git config --global credential.helper store
# 查看节点树
git log --oneline --graph --decorate --all

# 这个命令把分支"branchname"合并到了当前分支里面。
git merge branchname

#git 删除远程分支
git push origin :br

# 移除远程仓库
git remote rm origin

# 查看连接的远程仓库
git remote -v

# 添加一个远程仓库连接
git remote add origin https://github.com/aBadString/git-test.git

# 图状显示提交日志
git log --oneline --graph --decorate --all
```

**【当要提交】**

```bash
# 先切换到主分支
git checkout master

# 拉取远程仓库的代码
git pull origin master

# 把 SongDev 分支合并到 master
git merge SongDev

# 删除 本地 SongDev 分支
git branch -d SongDev
# 删除 远程 SongDev 分支  (origin 后面有空格)
git push origin :SongDev

# 建立新的 SongDev 继续开发
git branch SongDev
```


# 4. Git Commit message 编写指南

> 参考: http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html

当我们提交代码的时候, 必须要填写 Commit message (提交说明). 

```bash
git commit -m "hello world"
```

Commit message 包括三个部分：
- Header
- Body
- Footer

```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

## 4.1. Header 包括三部分
- type: 必须, 指明提交类型
- scope: 可选, 说明 commit 影响的范围, 比如数据层、控制层、视图层等等, 视项目不同而不同
- subject: 必须, 对提交的简短描述

type 的类型有以下7种:
- feat：新功能（feature）
- fix：修补 bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

