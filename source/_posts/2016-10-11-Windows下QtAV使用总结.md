---
title: Windows下QtAV使用总结
date: 2016-10-11 22:33:44
categories:
	- C/C++/Qt
tags:
	- QtAV
description: "QtAV"
---

# Windows下QtAV使用总结

参考[ubuntu上编译、安装，测试](http://blog.csdn.net/iloveqt5/article/details/38128867)

## 准备工作

+ 先集成 Qt开发环境
+ 配置 FFmpeg(Dev) （可以查看我的博客《Windows下配置FFmpeg》）
+ 配置 portaudio 我没有配置
+ 在 GitHub 下载 [QtAV](https://github.com/wang-bin/QtAV)

## MinGW编译 QtAV

参考 [Build-QtAV](https://github.com/wang-bin/QtAV/wiki/Build-QtAV)

+ 用 `Qt Creator` 打开 `QtAV.pro`
+ 选择 `项目` - `构建设置` - `构建环境`
+ 添加 `CPATH` = `...\ffmpeg path\include `
+ 添加 `LD_LIBRARY_PATH` = `...\ffmpeg path\lib `
+ 添加 `LIBRARY_PATH` = `...\ffmpeg path\lib `
+ 点击 `构建项目（Ctrl + b）`
+ 构建完成后，在 `构建环境` 中找到  `构建目录` 并打开
+ 用编辑器打开 `sdk_install.bat` 这个脚本是将本目录中的库与头文件等内容复制到你安装的Qt目录
+ 执行 `sdk_install.bat` 会将QtAv以一个模块的方式加入到你安装Qt的目录


## MSVC 编译 QtAV (先记录)

+ 用 `Qt Creator` 打开 `QtAV.pro`
+ 选择 `项目` - `构建设置` - `构建环境`
+ 修改 `INCLUDE` = `...\ffmpeg path\include; ...`
+ 修改 `LIB` = `...\ffmpeg path\lib; ...`
+ 修改 `PATH` = `...\ffmpeg path\bin;...\ffmpeg path\lib; ... `
+ 点击 `构建项目（Ctrl + b）`
+ 构建完成后，在 `构建环境` 中找到  `构建目录` 并打开
+ 用编辑器打开 `sdk_install.bat` 这个脚本是将本目录中的库与头文件等内容复制到你安装的Qt目录
+ 执行 `sdk_install.bat` 会将QtAv以一个模块的方式加入到你安装Qt的目录



## 使用QtAV

参考 [Use QtAV In Your Projects](https://github.com/wang-bin/QtAV/wiki/Use-QtAV-In-Your-Projects)

+ pro 中添加 `QT += avwidgets`
+ 头文件中添加
```
#include <QtAV>
#include <QtAVWidgets>
```

FFmpeg

+ 把 `dev版本` 文件夹中的 `Include` 和 `lib` 目录整个儿复制合并到 `MinGW` 目录下
+ 把 `share版本` 文件夹中 `bin` 目录下对应的所有dll复制合并到 `MinGW` 目录下