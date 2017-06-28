---
title: Windows下配置FFmpeg
date: 2016-10-11 22:33:44
categories:
	- 多媒体
tags:
	- FFmpeg
description: "Windows下配置FFmpeg"
copyright: true
---

# Windows下配置FFmpeg

## 下载

下载 [FFmpeg](http://ffmpeg.org/download.html) 

FFMPEG分为3个版本：Static，Shared，Dev。

+ Static里面只有3个应用程序：ffmpeg.exe，ffplay.exe，ffprobe.exe，每个exe的体积都很大，相关的Dll已经被编译到exe里面去了。
+ Shared里面除了3个应用程序：ffmpeg.exe，ffplay.exe，ffprobe.exe之外，还有一些Dll，比如说avcodec-54.dll之类的。Shared里面的exe体积很小，他们在运行的时候，到相应的Dll中调用功能。
+ Dev版本是用于开发的，里面包含了库文件xxx.lib以及头文件xxx.h，这个版本不包含exe文件。

## 配置

+ 解压到指定目录（不能有空格）如：`D:\ProgramFiles\FFmpeg`
+ 打开环境变量 `设置` - `系统` - `关于` - `系统信息` - `高级系统设置` - `环境变量`
+ 编辑 `Path`，添加 `D:\ProgramFiles\FFmpeg\bin`（win10里边不用写分号）
+ 打开命令提示符执行 `ffmpeg -verions`，查看是否配置成功

## 使用FFmpeg。

你需要使用命令才能够使用FFmpeg。具体资料自行查询。