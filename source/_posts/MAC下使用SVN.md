---
title: MAC 下使用 SVN
date: 2018-02-26 16:27:47
categories:
	- SVN
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

# 提交

commit 简写为 ci
`svn commit -m "注释" filename.xxx` 如果选择了保持锁，就使用 `-–no-unlock` 开关

`*` 表示全部文件
```
svn commit -m “提交当前目录下的全部在版本控制下的文件“ 
svn commit -m “提交我的测试用test.php“ test.php
```

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

# 合并

`svn merge -r 200:205 file.xxx` 将版本200与205之间的差异合并到当前文件，但是一般都会产生冲突，需要处理一下

---

参考
+ https://www.cnblogs.com/luckythan/p/4478706.html
