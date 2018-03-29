---
title: MAC 下使用 SVN
date: 2018-02-26 16:27:47
categories:
	- 开发工具
tags:
	- SVN
description: 
copyright: true
---

目前的公司居然用的 SVN, 好吧, 以前在 Windows 上用过, 找找怎么在 Mac 上用 😂😂😂

如果安装了 Xcode 就默认安装了 svn 直接使用吧

# checkout到本地目录
checkout 简写为 co
```
svn checkout http://路径(目录或文件的全路径)　[本地目录全路径] --username　用户名 --password 密码
svn checkout svn://路径(目录或文件的全路径)　[本地目录全路径]  --username　用户名 --password 密码
```

# 添加新的文件
```
svn add filename.xxx
svn add *.swift
```

## 问题 E200009

```
bogon:ARdemo syc$ svn add .
svn: warning: W150002: '/Users/syc/Documents/svn/ARdemo' is already under version control
svn: E200009: Could not add all targets because some targets are already versioned
svn: E200009: Illegal target for the requested operation
```

添加 `force`
`svn add . --force`

# 忽略文件
## 全局

配置忽略文件 `vi ~/.subversion/config`
如果 `.subversion` 目录不存在，请运行 `svn status` 命令，虽然此命令会失败，但是会为你创建所需要的文件。
找到 `global-ignores` 一行，去掉注释，并编辑

## 单个文件

设置编辑方式 `export SVN_EDITOR=vim`
终端输入 `svn propedit svn:ignore filename` 使用 vim 打开一个文件,编辑保存
我操作失败 😂😂😂
> bogon:RigourWechat syc$ svn propedit svn:ignore project.config.json
No changes to property 'svn:ignore' on 'project.config.json'
bogon:RigourWechat syc$ svn propedit svn:ignore project.config.json
svn: E200009: Cannot set 'svn:ignore' on a file ('/Users/syc/Documents/svn/01源代码/RigourWechat/project.config.json')




# 提交

commit 简写为 ci
`svn commit -m "注释" filename.xxx` 如果选择了保持锁，就使用 `-–no-unlock` 开关

`*` 表示全部文件
```
svn commit -m “提交当前目录下的全部在版本控制下的文件“ 
svn commit -m “提交我的测试用test.php“ test.php
```

# 还原本地修改

`svn revert [-R] filepath` 恢复原始未改变的工作副本文件 filepath可以是准备回滚的文件、目录，如果想把某个目录下的所有文件包括子目录都回滚，加上 `-R` 选项。

# 更新

`svn update`

# 删除

先本地删除,后提交

1. `svn delete file.xxx`
2. `svn ci -m "delete file"`

# 查看状态
`svn status [path]`

# lock

```
svn　lock　-m　“加锁备注信息文本“　[--force]　文件名 
svn　unlock　文件名
```

# 日志

`svn log file.xxx`

# 文件信息

`svn info file.xxx`

# 比较

`svn diff file.xxx` 将修改的文件与基础版本比较
`svn diff -r 200:201 file.xxx` (对版本200和版本201比较差异)

# 解决冲突

更新文件冲突

> bogon:01 syc$ svn update
> Updating '.':
> U    xxx/xxx.xx
> C    aa/aa.json
> Updated to revision 18403.
> Summary of conflicts:
>   Text conflicts: 1
> Conflict discovered in file 'RigourWechat/app.json'.
> Select: (p) postpone, (df) show diff, (e) edit file, (m) merge,
>         (mc) my side of conflict, (tc) their side of conflict,
>         (s) show all options: p
> Summary of conflicts:
>   Text conflicts: 1

```
输入p：稍后解决，新增3个文件(.mine / .rxxx / .rxxx)
输入df：显示不同之处；
输入e：编辑文件；
输入m：合并文件；
输入mc，选择我的代码作为最新版本；
输入tc：选择svn的代码作为最新版本；
输入s：显示所有选项
```

如果忘记有哪些冲突，使用 `svn resolve` 查看

解决冲突后执行 `svn resolved file.xxx` 移除工作副本的目录或文件的“冲突”状态. 再次提交.

# 合并

`svn merge -r 200:205 file.xxx` 将版本200与205之间的差异合并到当前文件，但是一般都会产生冲突，需要处理一下

---

参考
+ https://www.cnblogs.com/luckythan/p/4478706.html
