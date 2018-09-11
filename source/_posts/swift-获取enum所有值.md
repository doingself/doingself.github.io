
---
title: "swift 获取enum所有值"
date: 2018-09-06 22:33:44
categories:
	- IOS
	- Swift
tags:
	- enum
description: "获取枚举的所有值"
copyright: true
---

### 协议

```
public protocol EnumCollection: Hashable {
    static func cases() -> AnySequence<Self>
    static var allValues: [Self] { get }
}

public extension EnumCollection {
    
    public static func cases() -> AnySequence<Self> {
        return AnySequence { () -> AnyIterator<Self> in
            var raw = 0
            return AnyIterator {
                let current: Self = withUnsafePointer(to: &raw) { $0.withMemoryRebound(to: self, capacity: 1) { $0.pointee } }
                guard current.hashValue == raw else {
                    return nil
                }
                raw += 1
                return current
            }
        }
    }
    
    public static var allValues: [Self] {
        return Array(self.cases())
    }
}
```

### enum

```
enum ABC: String, CaseIterable {
    case a, b, c
}
```

### 使用

```
ABC.allCases.map { $0.rawValue }
```

### 鸣谢

+ https://theswiftdev.com/2017/10/12/swift-enum-all-values/
