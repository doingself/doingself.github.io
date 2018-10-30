---
title: "URL Encode Decode"
date: 2018-10-29 16:33:44
categories:
    - IOS
    - Swift
tags:
    - URL
description: ""
copyright: true
---

前言
在WEB前端开发，服务器后台开发，或者是客户端开发中，对URL进行编码是一件很常见的事情，但是由于各个年代的RFC文档中的内容一直在变化，一些年代久远的代码就对URL编码和解码的规则和现在的有一些区别。

在1994年订制的RFC1738文档中，对字符串中的除了- _ .之外的所有非字母数字字符都替换成百分号(%)后跟两位十六进制数，十六进制数中字母必须为大写。

在2005年定义的RFC3986中，将针对- _.~四个字符之外的所有非字母数字字符进行百分号编码。当然 根据URL的类型不同，有也一部分预留字符不需要进行编码，例如查询的URL中可以包含? /字符，不需要转义。更详细文档的可以查看RFC 3986。

### Swift url encode / decode

```
// encode
addingPercentEncoding(withAllowedCharacters: CharacterSet)
// decode
removingPercentEncoding
```

```
let str = "zh（哈！） @#$%^&<*>(en!)end"
var encodeStr: String!

/*
* CharacterSet
* urlHostAllowed: 被转义的字符有  "#%/<>?@\^`\{\|\}
* urlPathAllowed: 被转义的字符有  "#%;<>?[\]^`\{\|\}
* urlUserAllowed: 被转义的字符有   #%/<>?@\^`\{\|\}
* urlQueryAllowed: 被转义的字符有  "#%<>[\]^`\{\|\}
* urlPasswordAllowed 被转义的字符有 "#%/:<>?@[\]^`\{\|\}
*/

var hostAllowedSet = CharacterSet.urlHostAllowed
encodeStr = str.addingPercentEncoding(withAllowedCharacters: hostAllowedSet)!
print("hostAllowedSet \t\t= \(encodeStr!)")//zh%EF%BC%88%E5%93%88%EF%BC%81%EF%BC%89%20%40%23$%25%5E&%3C*%3E(en!)end

// 对 )! 编码
hostAllowedSet.remove(charactersIn: ")!")
encodeStr = str.addingPercentEncoding(withAllowedCharacters: hostAllowedSet)!
print("remove charactersIn \t= \(encodeStr!)")//zh%EF%BC%88%E5%93%88%EF%BC%81%EF%BC%89%20%40%23$%25%5E&%3C*%3E(en%21%29end


// 只对 )! 编码
let allowedCharacterSet = CharacterSet(charactersIn: ")!").inverted
encodeStr = str.addingPercentEncoding(withAllowedCharacters: allowedCharacterSet)
print("CharacterSet charactersIn = \(encodeStr!)")//zh%EF%BC%88%E5%93%88%EF%BC%81%EF%BC%89 @#$%^&<*>(en%21%29end

// c api
let cfstr = CFURLCreateStringByAddingPercentEscapes(
    nil,
    str as CFString,
    nil,
    "@)!" as CFString,
    CFStringBuiltInEncodings.UTF8.rawValue
)
print("CFString \t\t\t\t= \(cfstr!)") //zh%EF%BC%88%E5%93%88%EF%BC%81%EF%BC%89%20%40%23$%25%5E&%3C*%3E(en%21%29end
```

### Objective-C url encode
API调用都是一样的,不过网上流传的比较多的是用的C API

```
NSString *ciphertext = @"saf#*&";        
NSCharacterSet *set = [[NSCharacterSet characterSetWithCharactersInString:@"!*'();:@&=+$,/?%#[]"] invertedSet];
        
NSString *resultString = [ciphertext stringByAddingPercentEncodingWithAllowedCharacters: set];
```

### C API

```
NSString *ciphertext = @"saf#*&";
NSString *encodedStr = (NSString *)CFBridgingRelease(
	CFURLCreateStringByAddingPercentEscapes (
			kCFAllocatorDefault,
			(CFStringRef)ciphertext,
			NULL,
			CFSTR("!*'();:@&=+$,/?%#[]"),
			kCFStringEncodingUTF8
		)
	);
```

# 鸣谢
+ https://www.cnblogs.com/luoxiaofu/p/7110011.html
+ https://stackoverflow.com/questions/24551816/swift-encode-url
