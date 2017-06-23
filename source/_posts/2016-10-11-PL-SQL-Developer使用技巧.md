---
title: PL/SQL Developer使用技巧
date: 2016-10-11 22:33:44
categories:
	- 开发工具
tags:
	- PL/SQL
description: "PL/SQL Developer使用技巧"
---

# PL/SQL Developer使用技巧

## 执行光标所在行SQL语句

一个sql窗口中如果有多条sql语句，点击执行或者按`F8`，会全部执行，其实我们需要的只是实行其中的某一条。下面介绍一种方法可以执行光标所在行SQL语句
```
Tools-->Preferences-->Window Types-->SQL Window-->选中AutoSelect statement复选框
Tools->Preferences->Edit->Other 顺便把Highlight edit line也加上吧。
```

## 使用自定义快捷键

PL/SQL Developer也可以像其他IDE那样使用自定义快捷键提高编写代码效率，为开发者提供方便。 如我们平时在sql窗口中使用最频繁的 `select from` 我们就可以设置一个快捷键来简化 `select from` 的输入。

1. 建立一个文本文件 `shortcuts.txt`，并写入如下内容：
	```
	s = SELECT * FROM
	w = WHERE 1 = 1 AND
	sc = SELECT count(*) FROM
	```
	另存到 `PL/SQL Developer` 的安装路径下的 `~\PlugIns` 目录下

2. Tools-->Preferences-->User Interface-->Editor-->AutoReplace，选中`Enable`复选框，然后浏览文件选中之前创建的`shortcuts.txt`，点击`Apply`

3. 重启`PL/SQL Developer`，在sql窗口中输入`s+空格`，`w+空格`，`sc+空格`做测试

## 自动保存访问数据库的用户名和密码

Tools->Preferences->Oracle->Logon History，勾上 `Store history` 和 `Store with password`，点击`Apply`

重新登录后就可以记住用户名密码了

