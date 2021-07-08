---
layout: post
title: git workflow
date: 2021-07-08
categories: blog
tags: [develop]
description: 日常使用的git workflow。
---

### 0. 说明

- 总的操作原则是一个 `pull request` 只能有***一个*** `commit`，在所有修改操作中不能增加新的commit。

### 1. fork个人仓
- 在 gitee 网页上操作，将公共仓fork到个人仓库。

### 2. clone个人仓
```
git clone 个人仓地址
```
`git remote` 查看有哪些remote，若只有origin，则需要添加
```
git remote add upstream 公共仓地址如git@gitee.com:edahelper/west.git
```
- 此后所有的操作在本地个人仓进行，正常的迭代从步骤[3-开始迭代开发](#3-开始迭代开发)开始

### 3. 开始迭代开发
```
git checkout master
git fetch upstream
git merge upstream/master
git push origin master
```
- 确定此时的分支为master，如果checkout失败，说明当前分支有未保存的部分，使用 `git stash --include-untracked` 或者直接 `git add . & git commit` 处理好工作栈，已有commit的情况下可以 `git commit --amend` 避免新增commit。
- 同步公共仓最新的master分支，以进行后续开发
```
git checkout -b 开发分支名
```
- 从最新的master分支拉出一个新分支用于开发

### 4. 开发完成后
- 确认编译通过以及单元测试通过。
- `git add 相应内容` 
- `git commit`
  - 务必注意格式，格式描述在[commit.md](../checkin_guide/commit.md)，模板文件在[.gitcommit_template](../checkin_guide/.gitcommit_template) 

### 5. 同步公共仓最新代码
- 经过一段时间的开发，公共仓的代码已经有了新的commit，需要进行同步
- 首先确认当前在开发分支，执行
  ```
  git fetch upstream
  git rebase upstream/master
  ```
- 在某些情况下，rebase命令可以`避免`增加新的commit。
- 接下来将本地个人仓的修改同步到远程个人仓
  ```
  git push origin 开发分支名
  ```
  - 如果已有开发分支，且由于rebase产生的commit重排报错，则需要 `git push -f origin 开发分支名` 强制推送。

### 6. 提交pr
- 在个人仓的网页端，点击 `pull request` 新建pr，选择分支，描述里关联issue编号
- 完成后手动在评论里输入 `/retest` 触发ci，根据提示修改
- 等待review，根据review意见进行修改

### 7. 有修改怎么处理pr
- 在本地个人仓相应开发分支修改后
  - `git add 相应内容`
  - `git commit --amend` 不需要修改msg，完成后自动生成新的commit id覆盖上次的commit，保证这个分支只有一个新的commit
  - `git push -f origin 开发分支名` 强制推送到远程个人仓
- 此时查看网页pull request处
  - 最新状态是我们的强制推送
  - 需要重新输入`/retest`触发ci
  - 继续等待review