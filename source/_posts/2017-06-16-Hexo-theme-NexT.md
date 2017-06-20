
---
title: 使用 Hexo - NexT 主题
date: 2017-06-16 16:58:55
categories:
	- Hexo
tags:
	- Hexo
description: "使用 Hexo - NexT 主题" #你對本頁的描述 可以省略
---


# 使用 Hexo - NexT 主题

假定你已经成功安装了 Hexo，并使用 Hexo 提供的命令创建了一个站点。

在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml。 

+ 一份位于站点根目录下，主要包含 Hexo 本身的配置
+ 另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。

## 安装 NexT

将主题文件拷贝至站点目录的 themes 目录下， 然后修改下配置文件即可

1. 下载主题

	定位到Hexo目录下 `git clone https://github.com/iissnan/hexo-theme-next themes/next`

2. 使用主题

	修改站点根目录下 `_config.yml` 文件中的 `theme: next`

3. 验证主题

	启动 Hexo 本地站点，并开启调试模式（即加上 --debug）整个命令是 `hexo s --debug` 此时即可使用浏览器访问 `http://localhost:4000` 检查站点是否正确运行。	

## 主题设定

1. 外观

	修改主题目录下 `_config.yml` 文件中的 `scheme: Mist`
	Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观

	+ Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
	+ Mist - Muse 的紧凑版本，整洁有序的单栏外观
	+ Pisces - 双栏 Scheme，小家碧玉似的清新

2. 语言

	修改站点根目录下 `_config.yml` 文件中的 `language: zh-Hans`

	+ English	language: en
	+ 简体中文	language: zh-Hans

3. 菜单
	
	NexT 使用的是 Font Awesome 提供的图标

	+ 菜单项（key和链接) 修改主题目录下 `_config.yml` 文件中的 `menu: key:link` 其中 `key`  是一个名称，这个名称并不直接显示在页面上，她将用于匹配图标以及翻译。
	+ 菜单项的显示文本 `key` 翻译文本放置在 NexT 主题目录下的 `languages/你所使用的语言.yml`
	+ 菜单项对应的图标 修改主题目录下 `_config.yml` 文件中的 `menu_icons: ...`

4. 侧栏
	
	默认情况下，侧栏仅在文章页面（拥有目录列表）时才显示，并放置于右侧位置。
	
	+ 修改主题目录下 `_config.yml` 文件中的 `sidebar.position: left/right` 改变侧栏的位置
	+ 修改主题目录下 `_config.yml` 文件中的 `sidebar.display: always` 改变侧栏显示的时机

		+ post - 默认行为，在文章页面（拥有目录列表）时显示
		+ always - 在所有页面中都显示
		+ hide - 在所有页面中都隐藏（可以手动展开）
		+ remove - 完全移除

5. 头像

	修改主题目录下 `_config.yml` 文件中的 `avatar` 设置成头像的链接地址

	+ 放置在 `source/images/` 目录下 配置为：`avatar: /images/avatar.png`
	+ 完整的互联网 URI 配置为：`avatar: http://example.com/avatar.png`

6. 其他

	+ 修改站点根目录下 `_config.yml` 文件中的 `author` 设置昵称
	+ 修改站点根目录下 `_config.yml` 文件中的 `description` 你喜欢的一句签名作为站点描述
	+ 设置「动画效果」 修改主题目录下 `_config.yml` 文件中的 `use_motion: true/false`
	+ NexT 自带两种背景动画效果 修改主题目录下 `_config.yml` 文件中的  `canvas_nest` 或者 `three_waves` 

	侧边栏社交链接

	+ 链接 修改主题目录下 `_config.yml` 文件中的 `social: 显示文本: 链接地址` 
	+ 链接的图标 修改主题目录下 `_config.yml` 文件中的 `social_icons: 显示文本: Font Awesome 图标名称` 

	开启打赏功能 修改站点根目录下 `_config.yml` 中填入 微信 和 支付宝 收款二维码图片地址

	```
	reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
	wechatpay: /path/to/wechat-reward-image
	alipay: /path/to/alipay-reward-image	
	```

## 第三方服务集成

	评论、搜索、统计、分享



## 其他配置

### 添加标签页面

1. 新建页面

	执行命令：

	```
	cd myBlog
	hexo new page tags
	```

	输入命令后，在myBlog/source下会新生成一个新的文件夹tags，在该文件夹下会有一个index.md文件。
2. 设置页面类型

	在myBlog/source/tags/index.md中添加type: "tags"

3. 设置文章的tags

	当要为某一篇文章添加标签，只需在myBlog/source/_post目录下的具体文章的tags中添加标签即可 

	```
	---
	title: 使用 Hexo - NexT 主题
	date: 2017-06-16 16:58:55
	categories: 
		- Hexo
	tags:  [Hexo, 其他]
	description: "使用 Hexo - NexT 主题" #你對本頁的描述 可以省略
	---
	```

### 添加分类页面

1. 新建页面

	执行命令：

	```
	cd myBlog
	hexo new page categories
	```

	输入命令后，在myBlog/source下会新生成一个新的文件夹categories，在该文件夹下会有一个index.md文件。

2. 设置页面类型

	在myBlog/source/categories/index.md中添加type: "categories"

3. 设置文章的 categories

	当要为某一篇文章添加分类，只需在myBlog/source/_post目录下的具体文章的categories中添加分类即可

	```
	---
	title: 使用 Hexo - NexT 主题
	date: 2017-06-16 16:58:55
	categories: 
		- Hexo
	tags:  [Hexo, 其他]
	description: "使用 Hexo - NexT 主题" #你對本頁的描述 可以省略
	---
	```

### 添加关于我页面

1. 新建页面

	```
	cd myBlog
	hexo new page about
	```

	输入命令后，在myBlog/source下会新生成一个新的文件夹about，在该文件夹下会有一个index.md文件。

2. 修改about/index.md

	```
	---
	title: about
	date: 2016-11-15 19:08:50
	---
	## 关于我

	```

### 添加站内搜索(我失败了)

NexT主题支持集成 Swiftype、 微搜索、Local Search 和 Algolia,Swiftype和Algolia都只有一段时间的试用期，可以采用Hexo提供的Local Search,原理是通过hexo-generator-search插件在本地生成一个search.xml文件，搜索的时候从这个文件中根据关键字检索出相应的链接。

1. 安装 hexo-generator-search

	在myBlog执行：`npm install hexo-generator-search --save`

2. 安装 hexo-generator-searchdb

	在myBlog执行：`npm install hexo-generator-searchdb --save`

3. 启用搜索
	
	修改站点根目录下 `_config.yml` 文件，增加 `search`

	```
	search:
		  path: search.xml
		  field: post
		  format: html
		  limit: 10000
	```

### 添加站内搜索(成功)

	修改主题目录下 `_config.yml` 文件中的 `avatar` 设置成头像的链接地址
	


