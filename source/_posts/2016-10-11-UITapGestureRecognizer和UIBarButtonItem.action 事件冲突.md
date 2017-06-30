---
title: UITapGestureRecognizer 和 UIBarButtonItem.action 事件冲突
date: 2016-10-11 22:33:44
categories:
	- IOS
    - Swift
tags:
	- UIGestureRecognizerDelegate
description: "触摸事件冲突"
copyright: true
---

UITapGestureRecognizer 和 UIBarButtonItem.action 事件冲突
+ UITapGestureRecognizer在整个UIView上 
+ UIToolbar在UIView上

解决方案
实现 UIGestureRecognizerDelegate ，并设置 UITapGestureRecognizer 的 delegate
```
extension XXXX: UIGestureRecognizerDelegate {
    // MARK: UIGestureRecognizerDelegate

    /*
    此方法在window对象在有触摸事件发生时，
    调用gesture recognizer的touchesBegan:withEvent:方法之前调用，
    如果返回NO,则gesture recognizer不会看到此触摸事件。(默认情况下为YES).
    */
    func gestureRecognizer(
	    gestureRecognizer: UIGestureRecognizer, 
	    shouldReceiveTouch touch: UITouch
    ) -> Bool {
        /*
         UIView ---> UIToolbar ---> UIBarButtonItem ---> action
         touch.view is UIToolbarTextButton
         touch.view?.superview is UIToolbar
         */
        if touch.view?.superview is UIToolbar {
            return false
        }else{
            return true
        }
    }
}
```