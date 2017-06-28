---
title: Jekyll + GitHub
date: 2016-10-11 22:33:44
categories:
	- Web
tags:
	- Jekyll
	- GitHub
description: "Jekyll + GitHub"
copyright: true
---

# Github

GitHub 是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。

# GitHub Pages

[GitHub Pages](https://pages.github.com/)
Websites for you and your projects.
Hosted directly from your GitHub repository. Just edit, push, and your changes are live.
为您和您的项目的网站。
直接从您的GitHub存储库托管。 只需编辑，推送，您的更改即可生效。

# jekyll

[Jekyll](http://jekyllcn.com) 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 Markdown）和我们的 Liquid 渲染器转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。

## 准备安装
+ [Ruby](https://www.ruby-lang.org/en/downloads/)（including development headers, Jekyll 2 需要 v1.9.3 及以上版本，Jekyll 3 需要 v2 及以上版本）
+ [RubyGems](https://rubygems.org/pages/download)
+ [NodeJS](https://nodejs.org/), 或其他 JavaScript 运行环境（Jekyll 2 或更早版本需要 CoffeeScript 支持）。
+ [Python](https://www.python.org/downloads/) Jekyll 2 或更早版本）

## Linux, Unix, Mac OS 安装
`$ gem install jekyll`

## Windows 安装
安装 Jekyll 的最好方式就是使用 [RubyGems](https://rubygems.org/pages/download). 所有 Jekyll 的 gem 依赖包都会被自动安装
`$ gem install jekyll`

## 代码高亮
如果你是一个使用 Jekyll 的程序员，用 [Pygments](http://pygments.org/) 或者 [Rouge](https://github.com/jneen/rouge) 来支持代码高亮吧

## 版本

查看你的Jekyll版本
```
jekyll --version
gem list jekyll
```
你可以在RubyGem找到任何gem软件包的最新版本，同时也可以通过gem命令行工具来查看：

`gem search jekyll --remote`
这样你会查到名为jekyll的gem包，并且在方括号中显示最新版本。另一个检查本机是否是最新版本的办法是执行命令gem outdated， 它会显示出当前系统中所有需要更新的gem包列表，如果你的jekyll不是最新版本，执行命令：

`gem update jekyll`


# 搭建

## 创建Github仓库
+ 登录 Github
+ 点击 “+” 选择 `New repository`
+ 填写 `Repository name` 使用 “用户名.github.io”
+ 选择 `Public`
+ 勾选 `Initialize this repository with a README`
+ `Create repository` 创建

## 运行jekyll模板
+ 把 [jekyll 模板](http://jekyllthemes.org/) clone 到本地
+ `cd jekyll-theme` 进入jekyll模板
+ `jekyll serve --watch` 启动
+ `http://127.0.0.1:4000/jekyll-theme` 查看
+ 修改配置 `_config.yml` 参考[这里](http://jekyllcn.com/docs/structure/)

## 提交到自己的仓库中

+ `cd jekyll-theme` 进入jekyll模板
+ `git remote set-url origin https://...git` 更新/更改 URL
+ `git remote -v` 查看 origin 的地址

# 遇到的问题

1. sudo gem update --system
```
ERROR:  While executing gem ... (Errno::EPERM)
Operation not permitted - /usr/bin/update_rubygems
```

2. sudo gem update -n /usr/local/bin --system
3. gem install jekyll
```
ERROR:  While executing gem ... (Gem::FilePermissionError)
You don't have write permissions for the /Library/Ruby/Gems/2.0.0 directory.
```

4. sudo gem install jekyll
```
ERROR:  While executing gem ... (Errno::EPERM)
Operation not permitted - /usr/bin/safe_yaml
```

5. sudo gem install jekyll -n /usr/local/bin
```
ERROR:  Error installing jekyll:
invalid gem: package is corrupt, exception while verifying: undefined method `size' for nil:NilClass (NoMethodError) in /Library/Ruby/Gems/2.0.0/cache/ffi-1.9.17.gem
```

6. 删除 cache 目录

7. sudo gem install jekyll
```
ERROR:  While executing gem ... (Errno::EPERM)
Operation not permitted - /usr/bin/listen
```

8. sudo gem install jekyll -n /usr/local/bin

9. 安装成功
