---
title: "Git Flow 日常使用"
date: 2018-10-15 15:37
categories:
    - 开发工具
tags:
    - Git
description: "Git Flow 简单理解, 简单使用"
copyright: true
---

使用 git-flow 并不是必须的。当积攒了一定的使用经验后，很多团队会不再需要它了。当你能正确地理解工作流程的基本组成部分和目标的之后，你完全可以定义一个属于你自己的工作流程。

git-flow 的命令完全可以使用 git 命令代替 (虽然会有点复杂)

## 分支

+ `master` 打包发布
+ `develop` 开发测试
+ `feature/` 编码分支, 按功能业务从 develop 检出并创建分支, 完成后合并到 develop
+ `release/` 预发布版本, 基于 develop 创建, 完成后合并到 master 和 develop
+ `hotfix/` 基于 master 创建, 进行 bug 修复, 完成后合并到 master 和 develop

## 安装

`brew install git-flow`

## 初始化

`git flow init`

```
➜  aaa git flow init
Initialized empty Git repository in /Users/syc/Documents/aaa/.git/
No branches exist yet. Base branches must be created now.
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
➜  aaa git:(develop)
```

## feature/

从 develop 检出创建, 最终合并到 develop

### 开发一个新功能, 或者修复一个 bug

`git flow feature start branch-name`

从 develop 创建 fix-bug 修复功能

```
➜  aaa git:(develop) git flow feature start fix-bug
Switched to a new branch 'feature/fix-bug'

Summary of actions:
- A new branch 'feature/fix-bug' was created, based on 'develop'
- You are now on branch 'feature/fix-bug'

Now, start committing on your feature. When done, use:

     git flow feature finish fix-bug

➜  aaa git:(feature/fix-bug)
```

### 完成功能


`git flow feature finish branch-name`

完成后, 默认删除 fix-bug 分支, 合并到 develop

```
➜  aaa git:(feature/fix-bug) git flow feature finish fix-bug
Switched to branch 'develop'
Updating d9810e1..8049dc0
Fast-forward
    ...
Deleted branch feature/fix-bug (was 8049dc0).

Summary of actions:
- The feature branch 'feature/fix-bug' was merged into 'develop'
- Feature branch 'feature/fix-bug' has been removed
- You are now on branch 'develop'

➜  aaa git:(develop)
```

## 命令总结

```
git flow +  init (初始化)
         |- feature (新功能) +  start (开始) + branch-name (分支名称)
         |- release (发布)   |- finish (完成)
         |- hotfix (修复)    |- publish ()
                            |- pull ()
```

## 鸣谢

+ http://www.cnblogs.com/cnblogsfans/p/5075073.html
+ https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/git-flow
