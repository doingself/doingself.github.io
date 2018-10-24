---
title: "swift 日期转换工具类"
date: 2018-10-24 11:33:44
categories:
    - IOS
    - Swift
tags:
    - extension
    - enum
description: "enum / extensin 日期转换"
copyright: true
---


# 使用 enum 转换时间

也可以使用 class struct, 我这里使用 enum

```
// 时间转换工具类
enum DateUtil{
    // ...
}
extension DateUtil {
    enum DateFormat: String {
        case ymd = "yyyy-MM-dd"
        case ymdhms = "yyyy-MM-dd HH:mm:ss"
        case ymdhmsS = "yyyy-MM-dd HH:mm:ss.SSS"
        
        var formatter: DateFormatter {
            let dateFormatter = DateFormatter()
            dateFormatter.dateFormat = self.rawValue
            return dateFormatter
        }
        
        func dateFrom(string: String) -> Date? {
            return formatter.date(from: string)
        }
        
        func stringFrom(date: Date) -> String {
            let dateFormatter = DateFormatter()
            dateFormatter.dateFormat = self.rawValue
            return dateFormatter.string(from: date)
        }
        
    }
}
```

# 使用 extension 转换时间

```
// 扩展
extension Date {
    static let timeFormatter: DateFormatter = {
        let timeFormatter = DateFormatter()
        timeFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss "
        return timeFormatter
    }()
    var toString: String {
        return Date.timeFormatter.string(from: self)
    }
}
```

# 使用

```
let date = Date()

let dateStrByEnum = DateUtil.DateFormat.ymdhmsS.stringFrom(date: date)

let dateStrByExtension = date.toString
```
