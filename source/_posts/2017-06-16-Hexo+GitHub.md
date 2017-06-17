---
title: Hexo + GitHub
date: 2017-06-16 16:58:55
categories:  #文章分類目錄 可以省略
	- Hexo
tags: #文章標籤 可以省略
	- Hexo
description: "Hexo + GitHub" #你對本頁的描述 可以省略
---


# Hexo + GitHub

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装 Hexo

1. 安装 [Node.js](https://nodejs.org/en/) 

作用：用来生成静态页面的 到 [Node.js官网](https://nodejs.org/en/) 下载相应平台的最新版本，一路安装即可。

2. 安装 [Git](https://git-scm.com/) 

作用：把本地的 hexo内容 提交到 GitHub 上去

3. 申请 GitHub 

作用：是用来做博客的远程创库、域名、服务器

4. 安装 hexo

使用 npm 安装 Hexo 

`$ sudo npm install -g hexo` (我在MAC执行的这个)

`npm install -g hexo-cli`


## 使用 Hexo

1. 初始化

	+ 建立一个博客文件夹，并初始化博客，<folder>为文件夹的名称，可以随便起名字 `hexo init <folder>`
	+ 进入博客文件夹，<folder>为文件夹的名称 `cd <folder>`
	+ node.js的命令，根据博客既定的dependencies配置安装所有的依赖包 `npm install`

	```
	.
	├── node_modules
	|   └── ......
	├── scaffolds
	|   ├── draft.md
	|   ├── page.md
	|   └── post.md 
	├── source
	|   ├── _drafts
	|   └── _posts 
	├── themes
	|   └── landscape
	├── _config.yml
	└── package.json
	```

	+ _config.yml 网站的配置信息
	+ package.json 应用程序的信息
	+ scaffolds 模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。Hexo的模板是指在新建的markdown文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。
	+ source 资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
	+ themes 主题 文件夹。Hexo 会根据主题来生成静态页面。

2. 配置

	在 _config.yml 中修改大部份的配置。

	网站

	+ title	网站标题
	+ subtitle	网站副标题
	+ description	网站描述 主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词
	+ author	您的名字 文章的作者
	+ language	网站使用的语言
	+ timezone	网站时区。Hexo 默认使用您电脑的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。

	网址

	+ url	网址	
	+ root	网站根目录	
	+ permalink	文章的 永久链接 格式	:year/:month/:day/:title/
	+ permalink_defaults	永久链接中各部分的默认值	

	目录

	+ source_dir	资源文件夹，这个文件夹用来存放内容。	source
	+ public_dir	公共文件夹，这个文件夹用于存放生成的站点文件。	public
	+ tag_dir	标签文件夹	tags
	+ archive_dir	归档文件夹	archives
	+ category_dir	分类文件夹	categories
	+ code_dir	Include code 文件夹	downloads/code
	+ i18n_dir	国际化（i18n）文件夹	:lang
	+ skip_render	跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。	

	文章

	+ new_post_name	新文章的文件名称	:title.md
	+ default_layout	预设布局	post
	+ auto_spacing	在中文和英文之间加入空格	false
	+ titlecase	把标题转换为 title case	false
	+ external_link	在新标签中打开链接	true
	+ filename_case	把文件名称转换为 (1) 小写或 (2) 大写	0
	+ render_drafts	显示草稿	false
	+ post_asset_folder	启动 Asset 文件夹	false
	+ relative_link	把链接改为与根目录的相对位址	false
	+ future	显示未来的文章	true
	+ highlight	代码块的设置

	分类 & 标签

	+ default_category	默认分类	uncategorized
	+ category_map	分类别名	
	+ tag_map	标签别名	

	日期 / 时间格式

	+ date_format	日期格式	YYYY-MM-DD
	+ time_format	时间格式	H:mm:ss

	分页

	+ per_page	每页显示的文章量 (0 = 关闭分页功能)	10
	+ pagination_dir	分页目录	page

	扩展

	+ theme	当前主题名称。值为false时禁用主题
	+ deploy	部署部分的设置

3. 一些常用命令

	```
	// 新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。
	hexo init [folder]

	// 新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。
	hexo new [layout] <title>

	// 生成静态文件 简写为 hexo g（-d,--deploy 文件生成后立即部署网站）（-w,--watch 监视文件变动）
	hexo generate

	// 发表草稿
	hexo publish [layout] <filename>

	// 启动服务器。默认情况下，访问网址为： http://localhost:4000/。 'ctrl + c'关闭server
	hexo server

	// 部署网站。简写为：hexo d
	hexo deploy

	// 清除缓存文件 (db.json) 和已生成的静态文件 (public)。
	hexo clean

	// 列出网站资料。
	hexo list <type>

	// 显示 Hexo 版本。
	hexo version

	// 在安全模式下，不会载入插件和脚本。当您在安装新插件遭遇问题时，可以尝试以安全模式重新执行。
	hexo --safe

	// 在终端中显示调试信息并记录到 debug.log
	hexo --debug

	// 隐藏终端信息。
	hexo --silent

	// 自定义配置文件的路径，执行后将不再使用 _config.yml。
	hexo --config custom.yml

	// 显示 source/_drafts 文件夹中的草稿文章。
	hexo --draft

	// 自定义当前工作目录（Current working directory）的路径。
	hexo --cwd /path/to/cwd
	```

## 部署到 GitHub

一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。

1. 创建仓库
2. 克隆你的仓库到本地
3. 创建两个分支：master 与 hexo
4. 切换到 hexo 分支
5. 在根目录依次执行 `npm install hexo`、`hexo init`、`npm install` 
6. 安装`hexo-deployer-git插件`，在根目录运行：`npm install hexo-deployer-git --save`
7. 修改站点根目录下 `_config.yml` 文件中的 `Deployment` 部分
	```
	# Deployment
	## Docs: https://hexo.io/docs/deployment.html
	deploy:
	  type: git
	  repo: Clone with HTTPS Use Git or checkout with SVN using the web URL.
	  branch: master
	```
8. 提交到git，依次执行 `git add .`、`git commit -m "..."`、`git push origin hexo`
9. 执行 `hexo g -d` 生成网站并部署到GitHub的master上。


### 关于日常的改动流程

1. 在本地对博客进行修改（添加新博文、修改样式等等）后，提交到 hexo 分支
2. 执行 `hexo g -d` 生成网站并部署到GitHub的master上。

### 更换电脑

1. 克隆你的仓库到本地
2. 切换到 hexo 分支
3. 在根目录依次执行 `npm install hexo`、`npm install` 
4. 安装`hexo-deployer-git插件`，在根目录运行：`npm install hexo-deployer-git --save`
5. 在本地对博客进行修改（添加新博文、修改样式等等）后，提交到 hexo 分支
6. 执行 `hexo g -d` 生成网站并部署到GitHub的master上。

---

参考[这里](https://hexo.io/zh-cn/docs/setup.html)
参考[这里](https://www.zhihu.com/question/21193762)