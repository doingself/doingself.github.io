---
title: "class struct 与 json 互转"
date: 2018-09-06 22:33:44
categories:
    - IOS
    - Swift
tags:
    - json
description: "class struct 与 json 互转"
copyright: true
---

+ `class` 使用 `KVO` 将 JSON(既dictionary)转为 class
+ 实现 `Codable` 协议后 `JSONDecoder` 转换 (`class` 和 `struct` 都可以实现 `Codable` 协议)

```
import Foundation

let child: [String: String] = ["child_a": "aaa","childB": "bbb"]
let dict: [String: Any] = [
    "a_b": 123,
    "cd": "456",
    "child": child
]
// Any ---> Data ---> String
let data = try? JSONSerialization.data(withJSONObject: dict, options: JSONSerialization.WritingOptions.prettyPrinted)
let json = String(data: data!, encoding: String.Encoding.utf8)
print(json!)


@objcMembers class TestClass: NSObject{
    // json 节点与 class 属性一致
    var a_b: Any?
    var cd: Any?
    var child: Any?
    //var childModel: class? // 将 child --> class
    
    override init() {
        super.init()
    }
    init(dict: [String: Any]) {
        super.init()
        self.setValuesForKeys(dict)
    }
    override func setValue(_ value: Any?, forUndefinedKey key: String) {
        let str = String(format: "undefine key=%@, value=%@", key, String(describing: value))
        print(str)
    }
}


struct TestStruct: Codable {
    var ab: Int?
    var cd: String?
    var child: TestChildStruct?
    
    // json 节点与 struct 属性不一致，通过 enum 匹配
    enum CodingKeys: String, CodingKey {
        case a_b
        case cd
        case child
    }
    // 手动对应
    // json -> struct
    init(from decoder: Decoder) throws {
        // 所有属性必须手动实现，否则属性为nil
        let values = try decoder.container(keyedBy: CodingKeys.self)
        // enum.a_b ---> struct.ab
        ab = try values.decodeIfPresent(Int.self, forKey: .a_b)
        cd = try values.decodeIfPresent(String.self, forKey: .cd)
        child = try values.decodeIfPresent(TestChildStruct.self, forKey: .child)
    }
    // struct -> json
    func encode(to encoder: Encoder) throws{
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(self.ab, forKey: .a_b)
        try container.encode(self.cd, forKey: .cd)
        try container.encode(self.child, forKey: .child)
    }
}

struct TestChildStruct: Decodable, Encodable{
    var childA: String?
    var childB: String?
    
    // 自动转换
    enum CodingKeys: String, CodingKey {
        case childA = "child_a"
        case childB
    }
}

// json -> class
let cl = TestClass(dict: dict)
cl.a_b
cl.cd
cl.child

// Any ---> Data ---> String
// class、struct ---> json
do{
    // 此次报错, 程序终止
    let jsonData = try JSONSerialization.data(withJSONObject: cl, options: JSONSerialization.WritingOptions.prettyPrinted)
    let jsonStr = String(data: jsonData, encoding: String.Encoding.utf8)
}catch let err {
    print(err)
}

do{
    // json -> struct
    let decoder = JSONDecoder()
    let st = try decoder.decode(TestStruct.self, from: data!)
    st.ab
    st.cd
    let child = st.child
    child?.childA
    child?.childB
    
    // struct ---> json
    let encoder = JSONEncoder()
    let jsonData = try encoder.encode(st)
    let jsonStr = String(data: jsonData, encoding: String.Encoding.utf8)
    
    let jsonData2 = try JSONSerialization.data(withJSONObject: st, options: JSONSerialization.WritingOptions.prettyPrinted)
    let jsonStr2 = String(data: jsonData2, encoding: String.Encoding.utf8)
}catch let err {
    print(err)
}
```
