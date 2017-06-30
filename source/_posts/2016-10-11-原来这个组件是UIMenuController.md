---
title: 原来这个组件是UIMenuController
date: 2016-10-11 22:33:44
categories:
    - IOS
    - Swift
tags:
    - UIMenuController
description: "原来这个组件是UIMenuController"
copyright: true
---

![输入图片说明](https://static.oschina.net/uploads/img/201608/10153621_TaHo.png "在这里输入图片标题")

```

    /**
     长按事件 
     */
    func longPressGestureAction(sender:UILongPressGestureRecognizer){
        if sender.state != UIGestureRecognizerState.Began {
            return
        }
        print("longPressGestureAction+++++++++++++++++ \(sender.state.rawValue)")
        
        // 1. first responder
        v.becomeFirstResponder()
        
        // 2. 获取菜单
        let menu = UIMenuController.sharedMenuController()
        if menu.menuVisible == false{
            // 3. 设置自定义菜单
            let item = UIMenuItem(title: "啦啦啦", action: #selector(self.lalaAction(_:)))
            menu.menuItems = [ item ]
            
            // 4. 菜单显示位置
            menu.setTargetRect(v.bounds, inView: v)
            
            // 5. 显示菜单
            menu.setMenuVisible(true, animated: true)
        }
    }
    func lalaAction(sender:AnyObject?){
        
    }
    /// 具备成为第一响应者的资格
    override func canBecomeFirstResponder() -> Bool {
        return true
    }
    /**
     *  这个方法决定了MenuController的菜单项内容
     *  返回YES，就代表MenuController会有action菜单项
     */
    override func canPerformAction(action: Selector, withSender sender: AnyObject?) -> Bool {
        print("canPerformAction +++++ \(action)")
        // 判断 action 中包含的各个事件的方法名称, 对比上了才能显示
        if (action == #selector(NSObject.copy(_:)) || action == #selector(self.lalaAction(_:)))
        {
            return true
        }
        return false
    }
```