---
title: "LayoutKit 简单分析"
date: 2018-09-06 22:33:44
categories:
	- IOS
	- Swift
tags:
	- readme
description: "linkin LayoutKit 简单分析"
copyright: true
---

### LayoutKit 简单使用


[LayoutKit 官网](https://github.com/linkedin/LayoutKit)

#### 创建

使用 tableView / collectionView 初始化 `ReloadableViewLayoutAdapter`
如: `let reloadableViewLayoutAdapter = ReloadableViewLayoutAdapter(reloadableView: tableView)`

#### 设置 `dataSource` 和 `delegate`

```
tabView.dataSource = reloadableViewLayoutAdapter
tabView.delegate = reloadableViewLayoutAdapter
```

关于 `talbeView cellForRowAtIndexPath`

```
/// - Warning: Subclasses that override this method must call super
open func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let item = currentArrangement[indexPath.section].items[indexPath.item]
    let cell = tableView.dequeueReusableCell(withIdentifier: reuseIdentifier, for: indexPath)
    item.makeViews(in: cell.contentView)
    return cell
}
```

#### 关于数据来源

在封装数据的同时, 设置的单元格内的约束


```
// 封装数据
let profileCard = ProfileCardLayout(
    name: "ProfileCardLayout.name",
    connectionDegree: "ProfileCardLayout.connectionDegree",
    headline: "ProfileCardLayout.headline",
    timestamp: "ProfileCardLayout.timestamp = 5 minutes ago",
    profileImageName: "50x50"
)
let feedItems = [Layout](repeating: profileCard, count: 1000)

reloadableViewLayoutAdapter.reload(width: width, synchronous: synchronous, layoutProvider: { [weak self] in
    return [Section(header: nil, items: itemList, footer: nil)]
})
```

### 关于单元格布局

```
import LayoutKit

open class ProfileCardLayout: StackLayout<UIView> {

    public init(name: String, connectionDegree: String, headline: String, timestamp: String, profileImageName: String) {
        let labelConfig = { (label: UILabel) in
            label.backgroundColor = UIColor.yellow
        }

        let nameAndConnectionDegree = StackLayout(
            axis: .horizontal,
            spacing: 4,
            sublayouts: [
                LabelLayout(text: name, viewReuseId: "name", config: labelConfig),
                LabelLayout(text: connectionDegree, viewReuseId: "connectionDegree", config: { label in
                    label.backgroundColor = UIColor.gray
                }),
            ]
        )

        let headline = LabelLayout(text: headline, numberOfLines: 2, viewReuseId: "headline", config: labelConfig)
        let timestamp = LabelLayout(text: timestamp, numberOfLines: 2, viewReuseId: "timestamp", config: labelConfig)

        let verticalLabelStack = StackLayout(
            axis: .vertical,
            spacing: 2,
            alignment: Alignment(vertical: .center, horizontal: .leading),
            sublayouts: [nameAndConnectionDegree, headline, timestamp]
        )

        let profileImage = SizeLayout<UIImageView>(
            size: CGSize(width: 50, height: 50),
            viewReuseId: "profileImage",
            config: { imageView in
                imageView.image = UIImage(named: profileImageName)
            }
        )

        super.init(
            axis: .horizontal,
            spacing: 4,
            sublayouts: [
                profileImage,
                verticalLabelStack
            ]
        )
    }
}
```
