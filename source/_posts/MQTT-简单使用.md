---
title: MQTT简单使用
date: 2018-04-10 09:26:39
categories:
	- 协议
tags:
	- MQTT
description: 
copyright: true
---

# 简介

MQTT（Message Queuing Telemetry Transport，消息队列遥测传输协议），是一种基于发布/订阅（publish/subscribe）模式的“轻量级”通讯协议，该协议构建于TCP/IP协议上，由IBM在1999年发布。MQTT最大优点在于，可以以极少的代码和有限的带宽，为连接远程设备提供实时可靠的消息服务。作为一种低开销、低带宽占用的即时通讯协议，使其在物联网、小型设备、移动应用等方面有较广泛的应用。

MQTT是一个基于客户端-服务器的二进制的消息发布/订阅传输协议。MQTT协议是轻量、简单、开放和易于实现的，这些特点使它适用范围非常广泛。在很多情况下，包括受限的环境中，如：机器与机器（M2M）通信和物联网（IoT）。其在，通过卫星链路通信传感器、偶尔拨号的医疗设备、智能家居、及一些小型化设备中已广泛使用。

协议就是通信双方的一个约定，即，表示第1位传输的什么、第2位传输的什么……。在MQTT协议中，一个MQTT数据包由：固定头（Fixed header）、 可变头（Variable header）、 消息体（payload）三部分构成。

提供三种等级的服务质量：

+ `最多一次` 尽操作环境所能提供的最大努力分发消息。消息可能会丢失。例如，这个等级可用于环境传感器数据，单次的数据丢失没关系，因为不久之后会再次发送。
+ `至少一次` 保证消息可以到达，但是可能会重复。
+ `仅一次` 保证消息只到达一次。例如，这个等级可用在一个计费系统中，这里如果消息重复或丢失会导致不正确的收费

# 服务端

### 安装 mosquitto

`brew install mosquitto`

提示
```
mosquitto has been installed with a default configuration file.
You can make changes to the configuration by editing:
    /usr/local/etc/mosquitto/mosquitto.conf

To have launchd start mosquitto now and restart at login:
  brew services start mosquitto
Or, if you don't want/need a background service you can just run:
  mosquitto -c /usr/local/etc/mosquitto/mosquitto.conf
==> Summary
🍺  /usr/local/Cellar/mosquitto/1.4.14_2: 33 files, 629.9KB

```
### 配置

配置文件路径 `/usr/local/Cellar/mosquitto/1.4.14_2/etc/mosquitto/mosquitto.conf`

```
# bind_address ip-address/host name
bind_address 127.0.0.1

# Port to use for the default listener.
port 1883

allow_anonymous false  //禁止匿名登录  
password_file /usr/local/etc/mosquitto/pwdfile.example  // 帐号密码文件全路径
```

添加账号密码文件
```
aa:123
bb:123
```

执行命令 `bogon:~ syc$ mosquitto_passwd -U /usr/local/Cellar/mosquitto/pwdfile.example` 对帐号密码进行TLS加密

### 命令

+ 停止服务 `brew services stop mosquitto`
+ 启动服务 `brew services start mosquitto`
+ 重启服务 `brew services restart mosquitto`

### 客户端测试

下载测试工具 http://mqttfx.jensd.de/index.php/download

# 微信小程序测试

小程序中使用 MQTT

现有如下第三方库可供选择:

+ https://github.com/tennessine/paho.mqtt.wxapp
+ https://github.com/mqttjs/MQTT.js
+ https://github.com/M2M-IoT/mqtt-chat-wxapp
+ https://github.com/eclipse/paho.mqtt.javascript

# 鸣谢

+ mqtt https://blog.csdn.net/qq_28877125/article/details/78325003
+ mosquitto 安装使用 https://my.oschina.net/u/3729363/blog/1595070
+ mosquitto 配置 https://www.cnblogs.com/bluealine/p/8624180.html
+ https://zhuanlan.zhihu.com/p/24823848
