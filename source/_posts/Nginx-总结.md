---
title: Nginx 总结
date: 2018-04-17 15:42:59
categories:
	- 运维
tags:
	- Nginx
description: 
copyright: true
---

# 简介

Nginx (engine x) 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。

代理服务器 通常是指局域网内部的机器通过代理server发送请求到互联网上的server，代理server一般作用在client。

1. http服务器。Nginx是一个http服务可以独立提供http服务。可以做网页静态服务器。
2. 虚拟主机。可以实现在一台服务器虚拟出多个网站。例如个人网站使用的虚拟主机。
	+ 基于端口的，不同的端口
	+ 基于域名的，不同域名
3. 反向代理
4. 负载均衡。当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用nginx做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载高宕机而某台服务器闲置的情况。

# 安装

## MAC 环境安装

1. 使用 `brew -v` 检查是否安装了 `brew`
brew常用的命令：
```
brew search mysql : 搜索具体的程序包
brew install mysql : 安装具体的程序包
brew info mysql : 查看具体程序的信息
brew uninstall mysql : 卸载具体的应用（这里只是用mysql  作个例子）
```

2. 使用 `brew install nginx` 安装 `nginx`

## Windows 环境安装

1. 在 [官方网站](https://nginx.org/en/download.html) https://nginx.org/en/download.html 下载源码, 并解压
2. 运行 `nginx.exe`, 即可启动 Nginx 服务
3. 浏览器访问 `http://localhost:8080/`

# 配置

运行 `brew info nginx` 查看

```
bogon:~ syc$ brew info nginx
nginx: stable 1.13.12 (bottled), HEAD
HTTP(S) server and reverse proxy, and IMAP/POP3 proxy server
https://nginx.org/
/usr/local/Cellar/nginx/1.13.12 (23 files, 1.4MB)
  Poured from bottle on 2018-04-17 at 15:58:12
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/nginx.rb
==> Dependencies
Required: openssl ✔, pcre ✔
Optional: passenger ✘
==> Options
--with-passenger
	Compile with support for Phusion Passenger module
--HEAD
	Install HEAD version
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To have launchd start nginx now and restart at login:
  brew services start nginx
Or, if you don't want/need a background service you can just run:
  nginx
```


+ `/usr/local/etc/nginx/nginx.conf` 配置文件
+ `/usr/local/var/www` 服务器默认路径
+ `/usr/local/Cellar/nginx/1.xx.xx` 安装路径



# 使用

## 方式一

`sudo nginx` 启动
`nginx -s quit` 退出
`sudo nginx -s stop` 停止
`nginx -s reload` 重新加载
`nginx -t` 测试nginx.conf配置

## 方式二

输入 `ps -ef|grep nginx` 获取到nginx的进程号，注意是找到 `nginx: master` 的那个进程号，如下面的进程好是 39398

```
bogon:~ syc$ ps -ef|grep nginx
  502 39398     1   0  4:21下午 ??         0:00.00 nginx: master process nginx
  502 39399 39398   0  4:21下午 ??         0:00.00 nginx: worker process
  502 39537 35799   0  4:22下午 ttys002    0:00.00 grep nginx
```

+ `kill -HUP 39398` 重启
+ `kill -QUIT 39398` 从容的停止，即不会立刻停止
+ `Kill -TERM 39398` 立刻停止
+ `Kill -INT 39398` 立刻停止

# 鸣谢

+ https://www.cnblogs.com/liguangsunls/p/7087095.html
+ https://www.cnblogs.com/zhouxinfei/p/7862285.html
+ https://blog.csdn.net/u010372981/article/details/64128858
+ https://blog.csdn.net/LYmahang123/article/details/70686969