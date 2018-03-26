---
title: GitHub README中的 gif 制作
date: 2018-03-26 13:45:54
categories:
	- 开发工具
tags:
	- gif
description: "在 Mac 下制作 gif 并添加到 GitHub 项目的 readme 中"
copyright: true
---

### licecap

我使用了 [licecap](https://www.cockos.com/licecap/), 大家可以自行下载

LICEcap 是一款屏幕录制工具，支持导出 GIF 动画图片格式，轻量级、使用简单，启动软件后，会显示一个中间透明的窗口框，LICEcap 可以将框框范围内的屏幕内容变化全部捕捉录制下来并保存成 GIF 格式的动画图片，录制过程中可以随意改变录屏范围。

### 操作流程

1. 拖拽窗口至合适的位置, 并调整录制区域大小(既 licecap窗口大小)
2. 点击 右下角 `record...` 按钮
3. 选择 gif 要保存的位置及文件名
4. 注意左下角 3秒倒计时, 然后开始录制 licecap 窗口内的内容
5. 点击右下角 `stop` 停止录制,并保存为 gif 文件

### 添加到 readme

将 gif 文件添加到仓库中, 在 Markdown 中引用图片
`![image](https://github.com/doingself/IOSadaptation/blob/master/images/ios10-image0.gif)`
