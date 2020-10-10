---
title: 记录一次 Cocoapods 升级
date: 2020-10-10 22:33:44
categories:
	- 开发工具
tags:
	- Cocoapods
description: "记录一次 Cocoapods 升级"
copyright: true
---



# 记录一次 Cocoapods 升级

1.9.0 ---> 1.9.3

```
login: Sat Oct 10 11:41:07 on ttys003
 cityfruit@CityFruit-15-inch-deMacBook-Pro  ~  pod --version
1.9.0
 cityfruit@CityFruit-15-inch-deMacBook-Pro  ~  gem source -l
*** CURRENT SOURCES ***

https://rubygems.org/
 cityfruit@CityFruit-15-inch-deMacBook-Pro  ~  sudo gem install cocoapods
Password:
Fetching cocoapods-core-1.9.3.gem
Fetching cocoapods-1.9.3.gem
Successfully installed cocoapods-core-1.9.3
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /usr/bin directory.
 cityfruit@CityFruit-15-inch-deMacBook-Pro  ~  sudo gem install -n /usr/local/bin cocoapods
Successfully installed cocoapods-1.9.3
Parsing documentation for cocoapods-1.9.3
Installing ri documentation for cocoapods-1.9.3
Done installing documentation for cocoapods after 1 seconds
1 gem installed
 cityfruit@CityFruit-15-inch-deMacBook-Pro  ~  pod --version
1.9.3
```
