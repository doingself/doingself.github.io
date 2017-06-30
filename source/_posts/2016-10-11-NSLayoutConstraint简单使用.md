---
title: NSLayoutConstraint 简单使用
date: 2016-10-11 22:33:44
categories:
    - IOS
    - Swift
tags:
    - NSLayoutConstraint
description: "NSLayoutConstraint 简单使用"
copyright: true
---

# 定义
```
var aView: UIView!
var imgView: UIImageView!
var lab: UILabel!
```

# 创建
```
aView = UIView(frame: CGRectMake(10,70,300,200))
self.view.addSubview(aView)

imgView = UIImageView()
imgView.image = UIImage(named: "img")

lab = UILabel()
lab.text = "this is a UILabel"

aView.addSubview(imgView)
aView.addSubview(lab)

aView.layer.borderWidth = 1
imgView.layer.borderWidth = 1
lab.layer.borderWidth = 1
```

# 添加约束
```
/*
NSLayoutConstraint(
     item: AnyObject,
     attribute: NSLayoutAttribute,
     relatedBy: NSLayoutRelation,
     toItem: AnyObject?,
     attribute: NSLayoutAttribute,
     multiplier: CGFloat,
     constant: CGFloat
)
参数说明:
第一个参数:指定约束左边的视图view1
第二个参数:指定view1的属性attr1，上下左右等
第三个参数:指定左右两边的视图的关系relation，大于等于小于
第四个参数:指定约束右边的视图view2
第五个参数:指定view2的属性attr2，，上下左右等
第六个参数:指定一个与view2属性相乘的乘数multiplier
第七个参数:指定一个与view2属性相加的浮点数constant
 
这个函数的对照公式为:
view1.attr1 <relation> view2.attr2 * multiplier + constant
 
注意:
1.如果你想设置的约束里不需要第二个view，要将第四个参数设为nil，第五个参数设为NSLayoutAttributeNotAnAttribute
*/

// 设置imgView
//如果通过代码来设置Autolayout约束, 必须给需要设置约束的视图禁用autoresizing
imgView.translatesAutoresizingMaskIntoConstraints = false

// imgView 上边 等于 aView 上边 * 1 + 10
let topLayoutConstraint = NSLayoutConstraint(item: imgView, attribute: NSLayoutAttribute.Top, relatedBy: NSLayoutRelation.Equal, toItem: aView, attribute: NSLayoutAttribute.Top, multiplier: 10, constant: 10)
aView.addConstraint(topLayoutConstraint)
// 左边 10
let leftLayoutConstraint = NSLayoutConstraint(item: imgView, attribute: NSLayoutAttribute.Left, relatedBy: NSLayoutRelation.Equal, toItem: aView, attribute: NSLayoutAttribute.Left, multiplier: 3, constant: 10)
aView.addConstraint(leftLayoutConstraint)
// 高度 50
let heightLayoutConstraint = NSLayoutConstraint(item: imgView, attribute: NSLayoutAttribute.Height, relatedBy: NSLayoutRelation.Equal, toItem: nil, attribute: NSLayoutAttribute.NotAnAttribute, multiplier: 1, constant: 50)
imgView.addConstraint(heightLayoutConstraint)
// 宽度 50
let widthLayoutContraint = NSLayoutConstraint(item: imgView, attribute: NSLayoutAttribute.Width, relatedBy: NSLayoutRelation.Equal, toItem: nil, attribute: NSLayoutAttribute.NotAnAttribute, multiplier: 1, constant: 50)
imgView.addConstraint(widthLayoutContraint)

//如果通过代码来设置Autolayout约束, 必须给需要设置约束的视图禁用autoresizing
lab.translatesAutoresizingMaskIntoConstraints = false
// lab 头部 等于 imgView 的头部*2+10
let top = NSLayoutConstraint(item: lab, attribute: NSLayoutAttribute.Top, relatedBy: NSLayoutRelation.Equal, toItem: imgView, attribute: NSLayoutAttribute.Top, multiplier: 2, constant: 10)
// lab 左边 等于 imgView 右边 * 1 + 10
let left = NSLayoutConstraint(item: lab, attribute: NSLayoutAttribute.Left, relatedBy: NSLayoutRelation.Equal, toItem: imgView, attribute: NSLayoutAttribute.Right, multiplier: 1, constant: 10)
// lab 宽高
let width = NSLayoutConstraint(item: lab, attribute: NSLayoutAttribute.Width, relatedBy: NSLayoutRelation.LessThanOrEqual, toItem: nil, attribute: NSLayoutAttribute.NotAnAttribute, multiplier: 1, constant: 150)
let height = NSLayoutConstraint(item: lab, attribute: NSLayoutAttribute.Height, relatedBy: NSLayoutRelation.LessThanOrEqual, toItem: nil, attribute: NSLayoutAttribute.NotAnAttribute, multiplier: 1, constant: 50)
aView.addConstraints([top,left])
lab.addConstraints([width,height])
```

# 效果

![效果](https://static.oschina.net/uploads/img/201609/08115124_rl2R.png "效果")