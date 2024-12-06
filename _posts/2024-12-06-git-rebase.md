---
layout: post
title: Git rebase 
categories: Git
tags: ["协同开发"]
---

git rebase 两个常见的用法：
* 将一个分支的更改并入到另外一个分支中去
* 合并多个 commit 为一个 commit

## 将一个分支的更改并入到另外一个分支中去
### 为什么想要这样做？
* 保持历史整洁，不会出现复杂的合并记录
* 确保自己的改动是基于最新的代码

### 一图胜千言
![git rebase](/images/git-rebase.png)

rebase 操作会把 feat-xxx 分支里的每个 commit 取消掉，并且把它们临时保存为 patch，然后把 feat-xxx 分支更新到
最新的 master 分支，最后把保存的这些 patch应用到 feat-xxx 分支上

### 操作命令
``` sh 
git checkout feat-xxx
git rebase master
# 解决冲突
git add somefile
git rebase --continue
# 推送到远程仓库
git push origin --force-with-lease
```

## 合并多个 commit 为一个 commit
### 什么场景下需要这个操作？
在实际生产中，本地可能进行多次 commit，而为了保持 history 整洁，希望合并 "相同功能" 的多个 commit

### 命令解释
`-i（--interactive）` 弹出交互式的界面进行编辑合并
指令：
* p, pick   = 保留 commit
* r, reword = 保留 commit, 但想修改 commit message
* e, edit   = 保留 commit, 但想停下来修改一些内容（这时可以修改文件，添加新的改动）
* s, squash = 保留 commit, 但想将提交合并到上一个提交中（两个提交信息都会保留）
* f, fixup  = 同 "squash", 但想丢弃 commit message
* x, exec   = 想在某个 commit 后做一些检查，next line：`exec your command`
* d, drop   = 丢弃 commit

### 操作命令
``` sh
# rebase -i 有两种使用方式 
# 1. 从 HEAD 版本开始往过去数 n 个版本
git rebase -i HEAD~n
# 2. 操作 (start, end] 区间的 commit，注意是左开右闭
git rebase -i start end
# 后续就是常规的 编辑合并信息、提交信息、（处理冲突）、推送代码
```