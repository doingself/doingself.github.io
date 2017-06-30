---
title: UIControlEvents 个人总结
date: 2016-10-11 22:33:44
categories:
	- IOS
	- Swift
tags:
	- UIControlEvents
description: "UIControlEvents 个人总结"
copyright: true
---

# API
```
public struct UIControlEvents : OptionSetType {
    public init(rawValue: UInt)
    
    public static var TouchDown: UIControlEvents { get } // on all touch downs
    public static var TouchDownRepeat: UIControlEvents { get } // on multiple touchdowns (tap count > 1)
    public static var TouchDragInside: UIControlEvents { get }
    public static var TouchDragOutside: UIControlEvents { get }
    public static var TouchDragEnter: UIControlEvents { get }
    public static var TouchDragExit: UIControlEvents { get }
    public static var TouchUpInside: UIControlEvents { get }
    public static var TouchUpOutside: UIControlEvents { get }
    public static var TouchCancel: UIControlEvents { get }
    
    public static var ValueChanged: UIControlEvents { get } // sliders, etc.
    @available(iOS 9.0, *)
    public static var PrimaryActionTriggered: UIControlEvents { get } // semantic action: for buttons, etc.
    
    public static var EditingDidBegin: UIControlEvents { get } // UITextField
    public static var EditingChanged: UIControlEvents { get }
    public static var EditingDidEnd: UIControlEvents { get }
    public static var EditingDidEndOnExit: UIControlEvents { get } // 'return key' ending editing
    
    public static var AllTouchEvents: UIControlEvents { get } // for touch events
    public static var AllEditingEvents: UIControlEvents { get } // for UITextField
    public static var ApplicationReserved: UIControlEvents { get } // range available for application use
    public static var SystemReserved: UIControlEvents { get } // range reserved for internal framework use
    public static var AllEvents: UIControlEvents { get }
}
```

# 测试代码
```
let btn = UIButton(frame: CGRectMake( 20 , 80 , 100 , 50))
btn.layer.borderWidth = 1
btn.layer.masksToBounds = true
btn.layer.cornerRadius = 5
btn.setTitle("btn title", forState: UIControlState.Normal)
btn.setTitleColor(UIColor.blackColor(), forState: UIControlState.Normal)
self.view.addSubview(btn)

btn.addTarget(self, action: "TouchDownAction:", forControlEvents: UIControlEvents.TouchDown)
btn.addTarget(self, action: "TouchDownRepeatAction:", forControlEvents: UIControlEvents.TouchDownRepeat)
btn.addTarget(self, action: "TouchDragEnterAction:", forControlEvents: UIControlEvents.TouchDragEnter)
btn.addTarget(self, action: "TouchDragExitAction:", forControlEvents: UIControlEvents.TouchDragExit)
btn.addTarget(self, action: "TouchDragInsideAction:", forControlEvents: UIControlEvents.TouchDragInside)
btn.addTarget(self, action: "TouchDragOutsideAction:", forControlEvents: UIControlEvents.TouchDragOutside)
btn.addTarget(self, action: "TouchUpInsideAction:", forControlEvents: UIControlEvents.TouchUpInside)
btn.addTarget(self, action: "TouchUpOutsideAction:", forControlEvents: UIControlEvents.TouchUpOutside)
btn.addTarget(self, action: "TouchCancelAction:", forControlEvents: UIControlEvents.TouchCancel)
btn.addTarget(self, action: "PrimaryActionTriggeredAction:", forControlEvents: UIControlEvents.PrimaryActionTriggered)
```

```
func TouchDownAction(sender:AnyObject?){
    print("TouchDown Action")
}
func TouchDownRepeatAction(sender:AnyObject?){
    print("TouchDownRepeat Action")
}
func TouchDragEnterAction(sender:AnyObject?){
    print("TouchDragEnter Action")
}
func TouchDragExitAction(sender:AnyObject?){
    print("TouchDragExit Action")
}
func TouchDragInsideAction(sender:AnyObject?){
    print("TouchDragInside Action")
}
func TouchDragOutsideAction(sender:AnyObject?){
    print("TouchDragOutside Action")
}
func TouchUpInsideAction(sender:AnyObject?){
    print("TouchUpInside Action")
}
func TouchUpOutsideAction(sender:AnyObject?){
    print("TouchUpOutside Action")
}
func TouchCancelAction(sender:AnyObject?){
    print("TouchCancel Action")
}
func PrimaryActionTriggeredAction(sender:AnyObject?){
    print("PrimaryActionTriggered Action")
}
```

# 测试
## 单击执行顺序
```
TouchDown Action
TouchUpInside Action
PrimaryActionTriggered Action
```

## 连续点击
```
TouchDown Action
TouchUpInside Action
PrimaryActionTriggered Action
TouchDown Action
TouchDownRepeat Action
TouchUpInside Action
PrimaryActionTriggered Action
TouchDown Action
TouchDownRepeat Action
TouchUpInside Action
PrimaryActionTriggered Action
```

# 个人总结

在按钮内按下执行TouchDown，向外滑动执行TouchDragInside，滑到边界执行TouchDragExit，滑到边界外执行TouchDragOutside，再次滑到边界执行TouchDragEnter，滑到边界内执行TouchDragInside。

在按钮外松开执行TouchUpOutside

在按钮内松开执行TouchUpInside
