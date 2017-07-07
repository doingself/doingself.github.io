---
title: NSLayoutConstraint（VFL）简单使用
date: 2016-10-11 22:33:44
categories:
	- IOS
	- Swift
tags:
    - NSLayoutConstraint
    - VFL
description: "NSLayoutConstraint（VFL）简单使用"
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

# 约束
```
//VFL全称是Visual Format Language，翻译过来是“可视化格式语言”。
//
// H:[cancelButton(72)]-12-[acceptButton(50)]
// canelButton宽72，acceptButton宽50，它们之间间距12
//
// H:[wideView(>=60@700)]
// wideView宽度大于等于60point，该约束条件优先级为700（优先级最大值为1000，优先级越高的约束越先被满足）
//
// V:[redBox][yellowBox(==redBox)]
// 竖直方向上，先有一个redBox，其下方紧接一个高度等于redBox高度的yellowBox
//
// H:|-10-[Find]-[FindNext]-[FindField(>=20)]-|
// 水平方向上，Find距离父view左边缘默认间隔宽度，之后是FindNext距离Find间隔默认宽度；
// 再之后是宽度不小于20的FindField，它和FindNext以及父view右边缘的间距都是默认宽度。（竖线“|” 表示superview的边缘）
//
//NSLayoutConstraint.constraintsWithVisualFormat(
//format: String,
//options: NSLayoutFormatOptions,
//metrics: [String : AnyObject]?,
//views: [String : AnyObject]
//)
//
// format ：VFL语句
// opts ：约束类型
// metrics ：VFL语句中用到的具体数值
// views ：VFL语句中用到的控件

//如果通过代码来设置Autolayout约束, 必须给需要设置约束的视图禁用autoresizing
imgView.translatesAutoresizingMaskIntoConstraints = false
lab.translatesAutoresizingMaskIntoConstraints = false

let viewDict:[String:AnyObject] = ["aa":imgView , "bb":lab]
let metricsDict = ["space":10]
let hVFL = "H:|-space-[aa(50)]-space-[bb(<=150)]"
let hArrLayoutConstraint = NSLayoutConstraint.constraintsWithVisualFormat(hVFL, options: NSLayoutFormatOptions.AlignAllTop, metrics: metricsDict, views: viewDict)
aView.addConstraints(hArrLayoutConstraint)

let VVFL = "V:[aa(50)]-space-|"
let vArrLayoutConstraint = NSLayoutConstraint.constraintsWithVisualFormat(VVFL, options: NSLayoutFormatOptions.AlignAllLeft, metrics: metricsDict, views: viewDict)
aView.addConstraints(vArrLayoutConstraint)
```

# 效果

![效果](https://static.oschina.net/uploads/img/201609/08143838_M2EL.png "效果")