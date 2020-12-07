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








# CocoaPods 创建自己的公开库 私有库

公开库, 私有库 都包含 `.podspec` 文件
公开库直接发布在 Trunk, 可以通过 CocoaPods 搜索
私有库绑定了 `私有库 repo` 需要在 Podfile 中添加 repo 源, 才能被搜索

## CocoaPods 创建自己的公开库

[CocoaPods 官网](https://cocoapods.org)

### Trunk

查看自己是否注册过 Trunk `pod trunk me`

``` shell
 swift-syc@localhost ~ pod trunk me
[!] You need to register a session first.
```

注册 Trunk `pod trunk register doingself@163.com "shiyanchao" --verbose`

``` shell
✘ swift-syc@localhost ~ pod trunk register doingself@163.com "shiyanchao" --verbose
opening connection to trunk.cocoapods.org:443...
opened
starting SSL for trunk.cocoapods.org:443...
SSL established, protocol: TLSv1.2, cipher: ECDHE-RSA-AES128-GCM-SHA256
<- "POST /api/v1/sessions HTTP/1.1\r\nContent-Type: application/json; charset=utf-8\r\nAccept: application/json; charset=utf-8\r\nUser-Agent: CocoaPods/1.9.3\r\nAccept-Encoding: gzip;q=1.0,deflate;q=0.6,identity;q=0.3\r\nHost: trunk.cocoapods.org\r\nContent-Length: 68\r\n\r\n"
<- "{\"email\":\"doingself@163.com\",\"name\":\"shiyanchao\",\"description\":null}"
-> "HTTP/1.1 201 Created\r\n"
-> "Date: Tue, 27 Oct 2020 08:17:45 GMT\r\n"
-> "Connection: keep-alive\r\n"
-> "Strict-Transport-Security: max-age=31536000\r\n"
-> "Content-Type: application/json\r\n"
-> "Content-Length: 195\r\n"
-> "X-Content-Type-Options: nosniff\r\n"
-> "Server: thin 1.6.2 codename Doc Brown\r\n"
-> "Via: 1.1 vegur\r\n"
-> "\r\n"
reading 195 bytes...
-> "{\"created_at\":\"2020-10-27 08:17:46 UTC\",\"valid_until\":\"2021-03-04 08:17:46 UTC\",\"verified\":false,\"created_from_ip\":\"221.218.102.202\",\"description\":null,\"token\":\"baffc1ecfc4292278e2ca4605c933670\"}"
read 195 bytes
Conn keep-alive
[!] Please verify the session by clicking the link in the verification email that has been sent to doingself@163.com
 swift-syc@localhost  ~
 swift-syc@localhost  ~
 swift-syc@localhost  ~
 swift-syc@localhost  ~
 swift-syc@localhost  ~
 swift-syc@localhost  ~  pod trunk me
  - Name:     shiyanchao
  - Email:    doingself@163.com
  - Since:    October 27th, 02:17
  - Pods:     None
  - Sessions:
    - October 27th, 02:17 - March 4th, 2021 02:37. IP: 221.218.102.202
```

### .podspec 创建

#### 创建 .podspec 文件 方式一
克隆 GitHub 仓库后, 创建 .podspec 单文件

```
 swift-syc@localhost ~/Documents/syc/SycKit > pod spec create SycKit
Specification created at SycKit.podspec
```

#### 创建 .podspec 文件 方式二

克隆 GitHub 仓库后, 创建 项目及 .podspec 文件

```
# 根据提示选择 iOS / Swift 生成仓库及 .podspec 文件
pod lib create SycKit
```

### code tag

编码 ...
提交 ...
打 tag ...

- 查看所有 `git tag`
- 创建 `git tag 0.0.1`
- 创建 `git tag -a 1.0.0 -m '标签说明'`
- 删除 `git tag -d 0.0.1`
- 删除远端 `git push origin :refs/tags/0.0.1`
- 推送 `git push --tags`



### 发布 Trunk

- 校验 .podspec 文件    
`--verbose` 如果验证失败会报错误信息    
`pod spec lint SycKit.podspec --verbose`    
`pod spec lint SycKit.podspec --allow-warnings`    
出现 `SycKit.podspec passed validation.` 表示校验通过    

- 发布到 Trunk
`pod trunk push SycKit.podspec --allow-warnings`
```
--------------------------------------------------------------------------------
 🎉  Congrats

 🚀  SycKit (0.0.1) successfully published
 📅  November 4th, 07:17
 🌎  https://cocoapods.org/pods/SycKit
 👍  Tell your friends!
--------------------------------------------------------------------------------
```

### 使用

- `pod repo update`
- `pod search SycKit`

### 删除发布到 CocoaPods 上的 Pods


```
pod trunk delete DYFToast 2.0.0 #删除指定版本的 pods
pod trunk deprecate DYFToast #将 pods 设置为过期
```

## CocoaPods 创建自己的私有库

需要 2 个仓库
- 私有项目源码库
- 私有库 (需要与私有项目版本)

具体步骤

1. 创建私有项目
2. 创建 .podspec 文件
3. codeing + tag + push
4. 校验 .podspec 文件
  - 本地校验   `pod lib lint --allow-warnings`
  - 远程校验   `pod spec lint --allow-warnings`
5. 添加一个私有库并和项目地址做绑定 (MyRepo用来 存放所有私有库 各个版本的描述文件)
  `pod repo add MyRepo https://github.com/xxx/xxx.git`
  查看在 Finder 目录`cd ~/.cocoapods/repos`， 可以发现增加了一个 MyRepo 的储存库
  先更新下我们的版本库 `pod repo update MyRepo`
  推送podspec文件到索引库中
  `SycKit` 私有源码仓库
  `SycSpec` 私有索引仓库
  `pod repo push SycSpec SycKit.podspec --sources=http://xxx/xxx/SycSpec.git`

6. 新建一个项目进行验证
  需要在podfile文件中添加源地址（私人pod库指明你的版本库地址）
```
source ‘https://github.com/CocoaPods/Specs.git’
source ‘https://github.com/xxx/MyRepo.git’

platform :ios, '8.0'

target ‘MyPodTest’ do
use_frameworks!

pod "AFNetWorking" #公有库
pod 'XXX' #我们的私有库

end
```



