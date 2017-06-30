---
title: extension扩展属性
date: 2016-10-11 22:33:44
categories:
	- IOS
	- Swift
tags:
	- extension
description: "extension扩展属性"
copyright: true
---

# 背景

在UICollectionView中的Cell展示UIImageView，同时绑定UITapGestureRecognizer事件，传值section 和 item

# 定义
```
extension UIImageView {
    private struct definestruct{
        static var defineSection:Int = 0
        static var defineItem:Int = 0
    }
    var definesection:Int{
        get{
            return objc_getAssociatedObject(self, &definestruct.defineSection) as! Int
        }
        set(value){
            objc_setAssociatedObject(self, &definestruct.defineSection, value, objc_AssociationPolicy.OBJC_ASSOCIATION_RETAIN_NONATOMIC)
        }
    }
    var defineitem:Int{
        get{
            return objc_getAssociatedObject(self, &definestruct.defineItem) as! Int
        }
        set(value){
            objc_setAssociatedObject(self, &definestruct.defineItem, value, objc_AssociationPolicy.OBJC_ASSOCIATION_RETAIN_NONATOMIC)
        }
    }
}
```

# 使用
```
imgview.definesection = indexPath.section
imgview.defineitem = indexPath.item
```