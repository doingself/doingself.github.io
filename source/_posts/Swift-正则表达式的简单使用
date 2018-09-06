---
title: Swift 正则表达式的简单使用
date: 2018-09-06 22:33:44
categories:
	- IOS
	- Swift
tags:
	- 正则表达式
description: "IOS 正则表达式的简单使用"
copyright: true
---

```
/**
 正则表达判断是否含有结果值
 - parameter pattern: 一个字符串类型的正则表达式
 - parameter str: 需要比较判断的对象
 - returns: 返回布尔值判断结果
 - warning: 注意匹配到结果的话就会返回true，没有匹配到结果就会返回false
 */
func regex(pattern:String, str:String) -> Bool {
    do{
        let regex = try NSRegularExpression(pattern: pattern, options:[NSRegularExpression.Options.caseInsensitive])
        let resultNum = regex.numberOfMatches(in: str, options: NSRegularExpression.MatchingOptions(rawValue: 0) , range: NSMakeRange(0, str.count))
        if resultNum>=1 {
            return true
        }
    }catch let err{
        print(err)
    }
    return false
}

/**
 正则表达式获取目的值
 - parameter pattern: 一个字符串类型的正则表达式
 - parameter str: 需要比较判断的对象
 - imports: 这里子串的获取先转话为NSString的[以后处理结果含NS的还是可以转换为NS前缀的方便]
 - returns: 返回目的字符串结果值数组(目前将String转换为NSString获得子串方法较为容易)
 - warning: 注意匹配到结果的话就会返回true，没有匹配到结果就会返回false
 */
func regexGetSub(pattern:String, str:String) -> [String] {
    var subStr = [String]()
    let regex = try! NSRegularExpression(pattern: pattern, options:[NSRegularExpression.Options.caseInsensitive])
    let results: [NSTextCheckingResult] = regex.matches(in: str, options: NSRegularExpression.MatchingOptions.init(rawValue: 0), range: NSMakeRange(0, str.count))
    print(results)
    //解析出子串
    for  rst in results {
        let nsStr = str as NSString
        subStr.append(nsStr.substring(with: rst.range))
    }
    return subStr
}

```

使用

```
// 仅限字母和数字
let pattern = "^[A-Za-z0-9]+$"
regex(pattern: pattern, str: "2s3d") // true
regexGetSub(pattern: getPattern, str: "8e2D") // ["8e2D"]
```

鸣谢

+ https://blog.csdn.net/u011598999/article/details/79729229
+ https://blog.csdn.net/qq_14920635/article/details/77689830
