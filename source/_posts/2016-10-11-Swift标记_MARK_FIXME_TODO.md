---
title: Swift 标记 MARK FIXME TODO
date: 2016-10-11 22:33:44
categories:
	- IOS
    - Swift
tags:
	- MARK
	- FIXME
	- TODO
description: "Swift 标记 MARK FIXME TODO"
copyright: true
---

## 添加标记
```
// MARK: 注释说明

// FIXME: 此处有bug，需要优化

// TODO: 一般用于写到哪了 做个标记，然后回来继续
```

## 把 TODO 和FIXME 加上警告

+ 选择项目 - Targets - Build Phases
+ 点击 \+ 选择 New Run Script Phases 添加以下代码
```
TAGS="TODO:|FIXME:"
echo "searching ${SRCROOT} for ${TAGS}"
find "${SRCROOT}" \( -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TAGS).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/"
```
