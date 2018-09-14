---
title: Swift 枚举使用
date: 2018-09-14 22:33:44
categories:
	- IOS
	- Swift
tags:
	- enum
description: "enum"
copyright: true
---

### 枚举

```
enum Media {
    case Book(title: String, author: String, year: Int)
    case Movie(title: String, director: String, year: Int)
    case WebSite(url: NSURL, title: String)
}
let book = Media.Book(title: "20,000 leagues under the sea", author: "Jules Verne", year: 1870)

extension Media {
    var mediaTitle: String {
        switch self {
        case .Book(title: let aTitle, author: _, year: _), let .Movie(aTitle, _, _):
            return aTitle
        case .WebSite(let url, let title):
            return title
        }
    }
}
book.mediaTitle

extension Media {
    var isFromJulesVerne: Bool {
        switch self {
        case .Book(title: _, author: "Jules Verne", year: _): return true
        case .Movie(title: _, director: "Jules Verne", year: _): return true
        default: return false
        }
    }
}
book.isFromJulesVerne

extension Media {
    func checkAuthor(author: String) -> Bool {
        switch self {
        case .Book(title: _, author: author, year: _): return true
        case .Movie(title: _, director: author, year: _): return true
        default: return false
        }
    }
}
book.checkAuthor(author: "Jules Verne")


if case let Media.Book(title, _, _) = book {
    print("This is a book named \(title)")
}
// guard case let Media.Book(_, _, year) = book else { ... }
// for case let Media.Book(_, _, year) in array { ... }
if case let Media.Book(_, _, year) = book, year > 2000 {
    print("This is a book year \(year)")
}
```

### 鸣谢

+ http://swift.gg/2016/04/26/pattern-matching-1/
+ http://swift.gg/2016/04/27/pattern-matching-2/
+ http://swift.gg/2016/04/28/pattern-matching-3/
+ http://swift.gg/2016/06/06/pattern-matching-4/
