---
title: "swift5 画图"
date: 2019-10-23 15:13:44
categories: 
- IOS
- Swift
tags:
- UIGraphicsGetCurrentContext
description: "先记录, 再补充"
copyright: true
---

```

/*
_____
|   /
|  /
| /
|/
*/



/// 左上角上角三角形
class TriangleView: UIView {
    var stokeColor: UIColor = UIColor.red
    var fillColor: UIColor = UIColor.blue
    
    override func draw(_ rect: CGRect) {
        // 获取当前的图形上下文
        let context = UIGraphicsGetCurrentContext()
        // 设置边框颜色
        context?.setStrokeColor(stokeColor.cgColor)
        // 设置填充颜色
        context?.setFillColor(fillColor.cgColor)
        // 开始画线,需要将起点移动到指定的point
        context?.move(to: CGPoint(x: 0, y: 0))
        // 添加一根线到另一个点 (两点一线)
        context?.addLine(to: CGPoint(x: rect.maxY, y: 0))
        // 添加一根线到另一个点 (两点一线)
        context?.addLine(to: CGPoint(x: 0, y: rect.maxX))

        // 画线完毕,最后将起点和终点封起来
        context?.closePath()
        /*
            stroke      : 边框;
            fill        : 填充
            fillStroke  : 边框 + 填充
         */
        context?.drawPath(using: .fillStroke)
        // 渲染上下文
        context?.strokePath()
    }
}

let v = TriangleView(frame: CGRect(x: 0, y: 0, width: 300, height: 300))
```
