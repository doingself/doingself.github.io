---
title: Mac Launchpad 中的应用重复
date: 2016-10-11 22:33:44
categories:
	- 开发工具
tags:
	- Launchpad
description: "Launchpad 中的应用重复"
copyright: true
---

# Launchpad 中的应用重复

使用App Store更新后，Launchpad出现重复图标

将 launchpad恢复出厂设置，你在launchpad中建立的文件夹分类会丢失。

+ 打开终端
+ 输入 `defaults write com.apple.dock ResetLaunchPad -bool true`
+ 回车
+ 输入 `killall Dock`
+ 回车
+ 重新拖拽你的应用到 Launchpad
