---
title: "swift Codable"
date: 2019-12-26 11:33:44
categories: 
  - IOS
  - Swift
tags:
  - Codable
description: "Codable"
copyright: true
---


```
struct Person: Codable {
  var name: String
  var age: Int
  var quest: String
}

extension Person {
  private enum CodingKeys: CodingKey {
    case name = "person_name"
    case age
    case quest
  }
  
  /// model -> json
  func encode(to encoder:Encoder) throws {
    var container = encoder.container(keyedBy: CodingKeys.self)

    try container.encode(name, forKey: .name)
    try container.encode(age, forKey: .age)
    try container.encode(quest, forKey: .quest)
  }
  
  /// json -> model
  init(from decoder: Decoder) throws {
    let container = try decoder.container(keyedBy: CodingKeys.self)

    name = try container.decode(String.self, forKey: .name)
    age = try container.decode(Int.self, forKey: .age)
    quest = try container.decode(String.self, forKey: .quest)
  }
}
```
