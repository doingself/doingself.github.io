---
title: Sublime Text的JSON插件Pretty JSON
date: 2016-10-11 22:33:44
categories:
	- 开发工具
tags:
	- Sublime Text
	- 插件
	- JSON
description: "Sublime 格式化 JSON"
copyright: true
---

# Sublime Text的JSON插件Pretty JSON

## Sublime Text 安装

到[Sublime Text官网](http://www.sublimetext.com/3)下载，并安装
![image](2016-10-11-Sublime_Text的JSON插件Pretty_JSON/image1.jpg)

## 安装 `PackageControl`

1. 到 [这里](https://packagecontrol.io/installation) 复制代码
	![image](2016-10-11-Sublime_Text的JSON插件Pretty_JSON/image2.jpg)
	`import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)`

2. 打开 `Sublime -> View -> Show Console` 输入 复制的代码，并回车
	![image](2016-10-11-Sublime_Text的JSON插件Pretty_JSON/image3.jpg)

3. 重启后 `cmd + shift +p`，输入 `Install Package` 回车
	![image](2016-10-11-Sublime_Text的JSON插件Pretty_JSON/image4.jpg)

4. 打开窗口，搜索 `Pretty JSON` 回车，就可以使用  `Ctrl + Cmd + J` 格式化 `JSON`

---

参考的文章
1. http://blog.csdn.net/yanzi1225627/article/details/50703942
