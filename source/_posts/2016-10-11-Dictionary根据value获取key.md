---
title: Dictionary 根据value获取key
date: 2016-10-11 22:33:44
categories:
    - IOS
    - Swift
tags:
    - filter
description: "Dictionary 根据value获取key"
copyright: true
---

Dictionary 可以根据key获得value，可是如何根据value获取key呢？

以下是个人整理：

```
import UIKit
import Foundation

// http://stackoverflow.com/questions/27218669/swift-dictionary-get-key-for-value
// Dictionary 根据value获取key
extension Dictionary where Value : Equatable {
    func allKeysForValue(val : Value) -> [Key] {
        return self.filter { $1 == val }.map { $0.0 }
    }
    
    
    func someKeyFor(value: Value) -> Key? {
        guard let index = indexOf({ $0.1 == value }) else {
            return nil
        }
        return self[index].0
    }
}


func allKeysForValue222<K, V : Equatable>(dict: [K : V], val: V) -> [K] {
    return dict.filter{ $0.1 == val }.map{ $0.0 }
}

let dict = ["a" : 1, "b" : 2, "c" : 1, "d" : 2]

dict.allKeysForValue(1)

dict.someKeyFor(1)

allKeysForValue222(dict, val: 1)
```

---
参考文章：
http://stackoverflow.com/questions/27218669/swift-dictionary-get-key-for-value