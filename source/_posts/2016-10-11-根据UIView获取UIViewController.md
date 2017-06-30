---
title: 根据UIView获取UIViewController
date: 2016-10-11 22:33:44
categories:
	- IOS
	- Swift
tags:
	- UIResponder
description: "根据UIView获取UIViewController"
copyright: true
---

**根据UIView获取UIViewController**

```
if let supv = self.superview{
    if let nextres:UIResponder = supv.nextResponder() where nextres is UIViewController{
        let vc:UIViewController = nextres as! UIViewController
    }
}
```