---
title: "IOS 获取页面第一响应者"
date: 2018-09-06 22:33:44
categories: 
- IOS
- Swift
tags:
- UIResponder
description: ""
copyright: true
---

```
import UIKit    

extension UIResponder {
    private weak static var _currentFirstResponder: UIResponder? = nil

    public static var current: UIResponder? {
        UIResponder._currentFirstResponder = nil
        UIApplication.shared.sendAction(#selector(findFirstResponder(sender:)), to: nil, from: nil, for: nil)
        return UIResponder._currentFirstResponder
    }

    @objc internal func findFirstResponder(sender: AnyObject) {
        UIResponder._currentFirstResponder = self
    }
}
```

+ https://stackoverflow.com/questions/1823317/get-the-current-first-responder-without-using-a-private-api
