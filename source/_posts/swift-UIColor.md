---
title: "swift UIColor"
date: 2018-09-06 22:33:44
categories:
	- IOS
	- Swift
tags:
	- extension
  - UIColor
description: " 颜色扩展 rgb hex"
copyright: true
---

### 颜色扩展

根据 rgb 或 16进制 创建颜色
根据颜色获取 rgb 或 16进制

```
extension UIColor {

    public convenience init(r: CGFloat, g: CGFloat, b: CGFloat, a: CGFloat) {
        self.init(red: r / 255.0, green: g / 255.0, blue: b / 255.0, alpha: a)
    }
    
    public convenience init(hex: Int, alpha: CGFloat = 1.0) {
        let red = CGFloat(CGFloat((hex & 0xFF0000) >> 16)/255.0)
        let green = CGFloat(CGFloat((hex & 0x00FF00) >> 8)/255.0)
        let blue = CGFloat(CGFloat((hex & 0x0000FF) >> 0)/255.0)
        self.init(red: red, green: green, blue: blue, alpha: alpha)
    }
    
    public convenience init?(hexString: String, alpha: CGFloat = 1.0) {
        var formatted = hexString.replacingOccurrences(of: "0x", with: "")
        formatted = formatted.replacingOccurrences(of: "#", with: "")
        if let hex = Int(formatted, radix: 16) {
            self.init(hex: hex, alpha: alpha)
        } else {
            return nil
        }
    }

	// rgb
	// cicolor
    var redValue: CGFloat{
    	return CIColor(color: self).red
    }
    // cgcolor
    var greenValue: CGFloat{
    	let rgbColor = self.cgColor
    	return rgbColor.components[1]
    }
    // gerRed
	var blueComponent: Int {
        var b: CGFloat = 0
        getRed(nil, green: nil, blue: &b, alpha: nil)
        return Int(b * 255)
    }
    var alphaValue: CGFloat{
    	return CIColor(color: self).alpha
    }
    
    // 16进制
    func toHexString() -> String {
        var r:CGFloat = 0
        var g:CGFloat = 0
        var b:CGFloat = 0
        var a:CGFloat = 0
        getRed(&r, green: &g, blue: &b, alpha: &a)
        let rgb:Int = (Int)(r*255)<<16 | (Int)(g*255)<<8 | (Int)(b*255)<<0
        return String(format:"#%06x", rgb)
    }
}

```
