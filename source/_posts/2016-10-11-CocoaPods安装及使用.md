---
title: CocoaPods安装及使用
date: 2016-10-11 22:33:44
categories:
	- 开发工具
tags:
	- CocoaPods
description: "CocoaPods安装及使用"
---

# CocoaPods

CocoaPods是iOS的类库管理工具

CocoaPods 的原理，是将所有的依赖库都放到另一个名为 Pods 项目中，然后让主项目依赖 Pods 项目，这样，源码管理工作都从主项目移到了 Pods 项目中。

Pods 项目最终会编译成一个名为 libPods.a 的文件，主项目只需要依赖这个 .a 文件即可。
对于资源文件，CocoaPods 提供了一个名为 Pods-resources.sh 的 bash 脚本，该脚本在每次项目编译的时候都会执行，将第三方库的各种资源文件复制到目标目录中。
CocoaPods 通过一个名为 Pods.xcconfig 的文件来在编译时设置所有的依赖和参数。

## 安装
在安装CocoaPods之前，首先要在本地安装好Ruby环境。

### Ruby环境搭建

```
#查看当前ruby版本
ruby -v
#列出已知的ruby版本
rvm list known
#安装ruby 1.9.3
rvm install 1.9.3
```

### 下载和安装CocoaPods

+ 升级 gem
`sudo gem update --system`
+ 用淘宝的Ruby镜像来访问cocoapods
```
gem sources --remove https://rubygems.org/
// 等有反应之后再敲入以下命令
gem sources -a http://ruby.taobao.org/
// 验证你的Ruby镜像是并且仅是taobao
gem sources -l
```
+ 在 Terminator（也就是终端）中输入以下命令（注意，本文所有命令都是在终端中输入并运行的）
```
sudo gem install cocoapods
pod setup
```


## 使用

+ 在项目根目录中，使用 Vim 新建一个名为 Podfile 的文件，Podfile的内容是你想导入的类库
	```
	cd 项目根目录

	// 使用 Vim 新建一个名为 Podfile 的文件
	vim Podfile

	// 编辑后保存
	wq

	//获取类库
	pod install



	//我的Podfile 的文件内容如下：
	platform :ios, '9.0'
	inhibit_all_warnings!
	 
	target 'cocoDemo' do
	  pod 'MBProgressHUD', '~> 1.0.0'
	end
	```

+ 每次更改了 Podfile 文件，你需要重新执行一次 `pod update` 命令。
+ 使用 CocoaPods 生成的 `.xcworkspace` 文件来打开工程，而不是以前的 `.xcodeproj` 文件。
+ 在 `ProjectName-Bridging-Header.h` 头文件里写入要导入的 CocoaPods 库  `#import <MBProgressHUD/MBProgressHUD.h>` ，就可以在 Swift 中使用 `MBProgressHUD` 了

`pod install` 只会按照Podfile的要求来请求类库，如果类库版本号有变化，那么将获取失败。但是 `pod update` 会更新所有的类库，获取最新版本的类库。

### Podfile文件格式

```
source 'https://github.com/CocoaPods/Specs.git'

platform :ios, '6.0'
inhibit_all_warnings!

xcodeproj 'MyProject'

pod 'ObjectiveSugar', '~> 0.5'

target :test do
  pod 'OCMock', '~> 2.0.1'
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    puts #{target.name}
  end
end
```

+ source  'URL' ： 指定镜像仓库的源
+ platform : ios,  '6.0'  ： 指定所支持系统和最低版本
+ inhibit_all_warnings! ：屏蔽所有warning
+ workspace '项目空间名'： 指定项目空间名
+ xcodeproj '工程文件名'：指定xcodeproj工程文件名

pod 是声明指定依赖的方法

+ pod '库名',: 引入库，什么版本都可以（一般就是最新版本了）
+ pod '库名', '版本' ： 引入指定版本的库
+ \>   <   >=   <= 运算符可以指定版本的范围
+ ~ >  :  从指定版本到倒数第二位版本号升1为止，比如 '~> 1.2.1'是指  1.2.1 <= 版本 < 1.3.0
+ pod '库名', :podspec => 'podspec文件路径'  :  指定导入库的podspec文件路径
+ pod '库名', :git => '源码git地址'  :  指定导入库的源码git地址
+ pod '库名', :tag => 'tag名'  :  指定导入库的Tag分支


---


参考 [code4app cocoapods](http://code4app.com/article/cocoapods-install-usage)

参考 [Podfile Syntax Reference](http://guides.cocoapods.org/syntax/podfile.html#plugin)

