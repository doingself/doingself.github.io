---
title: MAC 下 iOS 环境搭建
date: 2018-02-26 16:27:47
categories:
	- 开发工具
tags:
	- 开发环境
description: 
copyright: true
---


Mac 环境搭建

# iTerm 配置主题

- https://www.jianshu.com/p/ba08713c2b19
- https://www.jianshu.com/p/60a2579af5fa

代理
git config --global --unset http.proxy
export http_proxy=http://127.0.0.1:1086/
export https_proxy=http://127.0.0.1:1086/
export socket5=http://127.0.0.1:1086/
export socket5=http://127.0.0.1:1086/
unset http_proxy
unset https_proxy


# Homebrew (brew) https://brew.sh/index_zh-cn

- https://zhuanlan.zhihu.com/p/111014448
- https://www.jianshu.com/p/2ca8a4e47dff



# Cocoapods https://cocoapods.org

- https://www.cnblogs.com/AnnieBabygn/articles/6494434.html
- https://www.jianshu.com/p/f43b5964f582

1. gem sources -l
2. gem sources --remove https://rubygems.org/
3. gem sources --add https://gems.ruby-china.com/
4. sudo gem update --system
5. sudo gem install cocoapods -n /usr/local/bin
6. pod setup
7. pod install


# Sublime Text

Command + Shift + P 安装 `Install Package`

## Pretty JSON (Command + Control + J)


# Alfred

- https://cn.bing.com/dict/search?q={query}
- https://www.baidu.com/s?wd={query}
- https://github.com/search?q={query}
- https://translate.google.cn/?sl=auto&tl=zh-CN&text={query}&op=translate



# Chrome 插件

- https://chrome.zzzmh.cn


# Charles

- https://www.charlesproxy.com/download/



# GitHub

- [Github Packages和Github Actions实践之CI/CD](https://www.cnblogs.com/yungyu16/p/12446571.html)



# 统计行号
`find . "(" -name "*.m" -or -name "*.mm" -or -name "*.cpp" -or -name "*.h" -or -name "*.swift" -or -name "*.rss" ")" -print | xargs wc -l`


# 镂空
```
let basicPath = UIBezierPath(rect: self.frame) // 底层
let maskPath = UIBezierPath(rect: targetView.frame) // 镂空层
basicPath.append(maskPath) // 重叠

let maskLayer = CAShapeLayer()
maskLayer.fillRule = CAShapeLayerFillRule.evenOdd // 奇偶层显示规则
maskLayer.path = basicPath.cgPath
self.layer.mask = maskLayer
```
# 笔记

1.iOS7: topLayoutGuide/bottomLayoutGuide，利用一个虚拟的view初步解决导航栏，tabbar的隔离问题。
2.iOS9:有了虚拟view的思路,又考虑能不能去除top/bottom概念的局限性，让开发者都可以灵活自定义这个隔离区域，又提供一些更方便简洁易懂的API方便进行代码自动布局，于是有了UILayoutGuide这个类。。
3.两年后的iOS11,有了iPhone X，苹果工程师顺理成章的将他们在iOS9的探索成果利用起来，他们自定义了一个UILayoutGuide，给开发者提供了一个只读属性的safeAreaLayoutGuide，并且提出安全区域的概念。
