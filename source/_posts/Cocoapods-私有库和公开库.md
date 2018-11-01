---
title: "Cocoapods 私有库 和 公开库"
date: 2018-11-01 14:03:04
categories:
	- 开发工具
tags:
	- Cocoapods
description: 
copyright: true
---

# 创建仓库

创建公有Pod库

+ 注册 Cocoapods 账号
+ 创建仓库
+ 仓库配置(version)
+ 上传到公有库
+ tag
+ 发布(trunk)

创建私有Pod库

+ 创建仓库
+ 仓库配置(version)
+ 上传到私有库
+ tag
+ 添加索引库
+ 发布(repo)

## 使用 Cocoapods 创建仓库

可以用于 `私有库`, 也可以用于 `公有库`, 关键看接下来提交的仓库, 及是否有索引库

`pod lib create MyLib`

进行相关选择后, 会自动执行 `pod install` 命令创建项目并生成依赖

## 配置

打开 `.../MyLib/Example/MyLib.xcworkspace` 工程
编辑 `MyLib` - `Podspec Metadata` - `MyLib.podspec` 文件, 配置信息

## 校验

执行 `pod lib lint MyLib.podspec` 校验配置是否有效

```
//分别进行验证检查本地和远程, 把警告忽略掉
pod lib lint --allow-warnings
pod spec lint --allow-warnings
```

## 编码

在 `Pods` - `Development Pods` - `MyLib` 中编码 (`MyLib/MyLib/Classes` 下)

## 提交到仓库

私有库提交到 `私有仓库`
公有库提交到 `公有仓库`

```
git add .
git commit -m "commit"
git remote add origin https://xxx.git
git push -u origin master
```

## 标记

在git仓库中创建一个与 `MyLib.podspec` 文件中 `s.version` 一致的 `tag`

```
git tag -a 1.0 -m "Release version 1.0"
git push origin --tags
```

# 公有库

将上述仓库替换为公开仓库

## 注册 Cocoapods

```
pod trunk register 邮箱地址 '用户名' --verbose
// 查看
pod trunk me
```

## 发布

将 `MyLib.podspec` 文件提交到公有的specs上面

`pod trunk push MyLib.podspec` 这一步做的操作是验证你的podspec文件是否合法+提交到specs中(等同于fork;commit;push)+将上传的podspec文件转成json格式文件)

## 使用

通过 `pod search Mylib` 查找并使用

## 更新迭代

如果有错误或者需要迭代版本,修改工程文件后推送到远端仓库后, 需要修改podspec中的版本号, 并重新打tag上传, 再进行新一轮的验证和发布, 当然, 创建一个演示demo工程供其他开发者下载查看并不会影响我们的pod库.

# 私有库

将上述仓库替换为私有仓库

## 添加索引库

存储私有库的索引文件
将索引库添加到本地的Cocoapods/repos目录下

```
cd ~/.Cocoapods/repos
//pod repo add 索引库名称 索引库地址
pod repo add MyLib https://git...git
// 查看
pod repo
```

## 放入索引库

公有库使用 `trunk` 方式将 `.podspec` 文件发布到CocoaPods/Specs
私有库使用 `repo`

```
//pod repo push 索引库名 podspec名
pod repo push MyLib MyLib.podspec
```

## 使用

`pod search MyLib`

需要在Podfile前面需要添加你的私有Spec repo的git地址source, pod install时, 才能在私有repo中查找到私有库

```
source 'https://github.com/CocoaPods/Specs.git'
source '索引库.git'

platform :ios, '7.0'
target "test" do
    pod 'MyLib', '~>1.0'
end
```

## 鸣谢
+ https://guides.cocoapods.org/making/using-pod-lib-create.html
+ https://www.jianshu.com/p/ea34acffa265
+ https://segmentfault.com/a/1190000007947371##articleHeader2
