---
title: 时间戳 NSTimeInterval
date: 2016-10-11 22:33:44
categories:
	- IOS
    - Swift
tags:
	- NSTimeInterval
description: "时间戳 NSTimeInterval"
copyright: true
---

```
//时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。

//获取当前时间
let now = NSDate()

// 创建一个日期格式器
let fmt = NSDateFormatter()
fmt.dateFormat = "yyyy年MM月dd日 HH:mm:ss"
print("当前日期时间：\(fmt.stringFromDate(now))")

//当前时间的时间戳
let timeInterval:NSTimeInterval = now.timeIntervalSince1970
let timeIntervalInt = Int(timeInterval)
print("当前时间的时间戳：\(timeIntervalInt)  ++  \(timeInterval)")

//将时间戳转为日期时间
let date = NSDate(timeIntervalSince1970: timeInterval)
print("对应的日期时间：\(fmt.stringFromDate(date))")


// 当前时间， 60*60（1小时）后的时间
let otherDate = NSDate(timeInterval: 60*60, sinceDate: now)
print("1小时后 日期时间：\(fmt.stringFromDate(otherDate))")

// 时间戳
// 晚 timeIntervalSinceDate 早 === 晚 - 早 === 正数 === 间隔秒钟
let difference = otherDate.timeIntervalSinceDate(now)
print("间隔秒钟：\(difference)")
```

**时间戳转分钟**
```
// 天 时 分 秒
var timeInterval = (60*60*24*2) + (60*60*5) + (60*3) + (20)
// 时间戳 --> 分钟
var dateStr = ""
if timeInterval >= 60*60*24 {
    let v = timeInterval / (60*60*24)
    timeInterval -= (60*60*24) * v
    dateStr += "\(v)天"
}
if timeInterval >= 60*60 {
    let v = timeInterval / (60*60)
    timeInterval -= (60*60) * v
    dateStr += "\(v)小时"
}
if timeInterval >= 60 {
    let v = timeInterval / (60)
    timeInterval -= (60) * v
    dateStr += "\(v)分"
}
dateStr += "\(timeInterval)秒"
print("\(dateStr)") // 2天5小时3分20秒
```