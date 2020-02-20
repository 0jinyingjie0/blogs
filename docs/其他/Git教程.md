# Git教程

## Git介绍

- Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
- Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
- Git 完整命令手册地址：<http://git-scm.com/docs>
- PDF 版命令手册：[github-git-cheat-sheet.pdf](https://www.runoob.com/manual/github-git-cheat-sheet.pdf)
- Git安装教程：[菜鸟](<https://www.runoob.com/git/git-install-setup.html>)

## Git 与 SVN 区别

- **git是分布式的，svn是集中式的。**
- **Git 把内容按元数据方式存储，而 SVN 是按文件**
- **Git 分支和 SVN 的分支不同**
- **Git 没有一个全局的版本号，而 SVN 有**
- **Git 的内容完整性要优于 SVN**

## Git基本过程

### 基本提交

- git init:仓库初始化
- git add . :把更新的内容提交到
- git commit -m "提示信息"

### 下载

- git clone 地址

### 查看状态

- git status:查看状态

### git diff

- 执行 git diff 来查看执行 git status 的结果的详细信息
- git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别
- git status 显示你上次提交更新后的更改或者写入缓存的改动， 而 git diff 一行一行地显示这些改动具体是啥。

### git reset HEAD

- git reset HEAD 文件名： 命令用于取消已缓存的内容。

### git rm

- 如果只是简单地从工作目录中手工删除文件，运行 **git status** 时就会在 **Changes not staged for commit** 的提示。

## 分支管理

- 创建分支：git branch 分支名字
- 切换分支：git checkout 分支名字
- 合并分支：git merge 子分支的名字
- 列出分支：git branch
- 删除分支：git branch -d 分支名字
- 合并冲突：手动解决

## 查看提交历史

- git log:查看历史提交记录
- git log --oneline：历史记录一行显示

## 标签

- 如果你达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。

```js
$ git tag -a v1.0 
```

## 远程仓库

- git remote add origin 地址
- git push -u origin master
- 下次提交直接Git push
- 待续。。。

