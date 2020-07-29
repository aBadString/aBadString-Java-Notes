<h1 align="center">Git</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. Git常用命令](#1-git常用命令)
- [2. Git Commit message 编写指南](#2-git-commit-message-编写指南)
  - [Header 包括三部分](#header-包括三部分)

<!-- /code_chunk_output -->

# 1. Git常用命令

```bash
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
# 可以通过如下命令进行代码合并【注：pull=fetch+merge]
git pull --rebase origin master

git config --global credential.helper store
# 查看节点树
git log --oneline --graph --decorate --all

# 这个命令把分支"branchname"合并到了当前分支里面。
git merge branchname

#git 删除远程分支
git push origin :br 

git remote add origin https://github.com/aBadString/git-test.git

git log --oneline --graph --decorate --all

[remote "origin"]
	url = https://github.com/aBadString/git-test.git
	fetch = +refs/heads/*:refs/remotes/origin/*
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


# 2. Git Commit message 编写指南

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

## Header 包括三部分
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

