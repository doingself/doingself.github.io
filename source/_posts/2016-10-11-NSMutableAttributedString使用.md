---
title: NSMutableAttributedString 使用
date: 2016-10-11 22:33:44
categories:
    - IOS
    - Swift
tags:
    - NSMutableAttributedString
description: "将聊天消息中的特殊字符替换为表情图标"
copyright: true
---

总结下，这几天对NSMutableAttributedString的使用

将聊天消息中的特殊字符替换为表情图标

```
import UIKit
import Foundation

func getFaceIcon(str:String) -> UIImage? {
    if str == "aa" || str == "cc"{
        return UIImage()
    }else{
        return nil
    }
}

func strToNSAttributeStr(contentstr:String , muattstr:NSMutableAttributedString) -> NSMutableAttributedString {
    // 填充真实内容
    let attstr = NSAttributedString(string: contentstr)
    muattstr.appendAttributedString(attstr)
    return muattstr
}

func textAndFaceImage(contentstr:String , muattstr:NSMutableAttributedString = NSMutableAttributedString() ) -> NSMutableAttributedString {
    //查找表情开始的地方
    if let startr = contentstr.rangeOfString("[") {
        // 截取表情
        let startstr = contentstr.substringFromIndex(startr.endIndex)
        //查找表情结束的地方
        if let endr = startstr.rangeOfString("]") {
            //截取到表情
            let endstr = startstr.substringToIndex(endr.startIndex)
            
            //获得真实的表情image
            if let faceimg:UIImage = getFaceIcon(endstr) {
                //表情 及 相关设置
                let attach:NSTextAttachment = NSTextAttachment()
                attach.image = faceimg
                let labheight:CGFloat = 30 //lab.font.lineHeight
                attach.bounds = CGRectMake(0, -5, labheight, labheight)
                let faceattstr:NSAttributedString = NSAttributedString(attachment: attach)
                
                // 填充表情前的内容
                let contentstrstart = contentstr.substringToIndex(startr.startIndex)
                muattstr.appendAttributedString(NSAttributedString(string: contentstrstart))
                
                // 填充表情
                muattstr.appendAttributedString(faceattstr)
                
                //表情后的内容
                let contentstrend = startstr.substringFromIndex(endr.endIndex)
                
                // 如果还有其他表情，继续替换，没有则结束
                if let _ = contentstrend.rangeOfString("[") {
                    return textAndFaceImage(contentstrend, muattstr: muattstr)
                }else{
                    // 填充表情后的内容
                    muattstr.appendAttributedString(NSAttributedString(string: contentstrend))
                    return muattstr
                }
            }else{
                // 填充表情前的内容
                let otherstrstart = contentstr.substringToIndex(startr.endIndex)
                muattstr.appendAttributedString(NSAttributedString(string: otherstrstart))
                //继续检索表情
                return textAndFaceImage(startstr, muattstr: muattstr)
            }
        }
    }
    return strToNSAttributeStr(contentstr, muattstr: muattstr)
}

// 原内容
let contentstr:String = "111[222[aa]3333]444[bb]5555[cc]666[777[aa]888]999"
// 真实内容
let muattstr:NSMutableAttributedString = textAndFaceImage(contentstr)

// 使用
lab.attributedText = muattstr
let labsize = lab.sizeThatFits(CGSizeMake(contentwidth, CGFloat.max))
```