---
title: 键盘遮挡UITableViewCell中的文本框
date: 2016-10-11 22:33:44
categories:
	- IOS
    - Swift
tags:
	- 键盘遮挡
description: "键盘遮挡UITableViewCell中的文本框"
copyright: true
---

当 `cell` 里面有 `textfield` 或者 `textview` 键盘弹起的时候，输入框就被遮挡了

使用 `UITableViewController` 来代替 `UIViewController`，`UITableViewController` 中的 `tableview` 已经做了可以自适应键盘高度

```
let tvc = UITableViewController(style: UITableViewStyle.grouped)
self.addChildViewController(tvc)
tvc.view.frame = self.view.frame
tabView = tvc.tableView

tabView.delegate = self
tabView.dataSource = self
```
