---
title: Sublime Text 的 Markdown 插件 Markdown Preview 
date: 2016-10-11 22:33:44
categories:
	- 开发工具
tags:
	- Sublime Text
	- 插件
	- Markdown
description: "Sublime Text 的 Markdown 插件 Markdown Preview "
copyright: true
---

# Sublime Text 的 Markdown 插件 Markdown Preview

## 安装

*前提是已经安装 Package Control*

+ 按下键 `Ctrl + Shift + P` 调出命令面板
+ 找到 `Package Control: install Pakage` 这一项，回车
+ 搜索 `markdown preview` 安装

## 使用

markdown preview默认没有快捷键，设置快捷键在`Preferences -> Key Bindings User`打开的文件的中括号中添加以下代码(可在Key Bindings Default找到格式)：


```
{ "keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"} }
```

`"Alt + M"` 可设置为自己喜欢的按键。

## 设置语法高亮和mathjax支持

在 `Preferences ->Package Settings->Markdown Preview->Setting Default` 中找到

```
/*
Enable or not mathjax support.
*/
"enable_mathjax": false,

/*
Enable or not highlight.js support for syntax highlighting.
*/
"enable_highlight": false,
```

将 `false` 改为 `true` 即可。

语法高亮跟编辑器的主题有关，可以在 `Preferences ->Color Scheme` 找自己喜欢的主题。

`MathJax` 是一个开源的基于 `Ajax` 的数学公式显示的解决方案，结合多种先进的Web技术，支持主流的浏览器。`MathJax` 根据页面中定义的 `LaTex` 数据，生成对应的数学公式。

