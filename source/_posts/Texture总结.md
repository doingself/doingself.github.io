---
title: "Texture总结"
date: 2018-12-27 20:33:44
categories:	
	- IOS
	- Swift
tags:
	- pod
description: "第三方库Texture总结"
copyright: true
---


# [TextureGroup/Texture](https://github.com/TextureGroup/Texture)

Texture(AsyncDisplayKit)是一款基于UIKit构建的iOS框架，其目的在于使复杂的界面依然能够保持流畅和响应。   
Node是Texture的基本单位，ASDisplayNode是对UIVew的抽象对象，而UIView是对CALayer的抽象。   
node是线程安全的，既可以在后台线程上实例化和配置它们的整个层次结构。   
UIView和CALayer的大多数属性在Node中亦存在，且命名相同。

![image](https://user-gold-cdn.xitu.io/2017/11/23/15fe8a510eed3c91?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
[原图地址](http://texturegroup.org/static/images/node-hierarchy.png)

## Node 容器

| Node Containers | UIKit Equivalent |
| --- | --- |
| ASCollectionNode | UICollectionView |
| ASPagerNode | UIPageViewController |
| ASTableNode | UITableView |
| ASViewController | UIViewController |
| ASNavigationController | UINavigationController |
| ASTabBarController | UITabBarController |

### ASViewController

#### init
这个方法在 ASViewController 的生命周期开始时被调用一次，与 UIViewController 的初始化一样，你最好不要在这个方法中访问 self.view 或  self.node.view，因为这样会强制创建 view。 这些操作可以在 -viewDidLoad 中进行，-viewDidLoad 可以执行任何 view 的访问。

#### loadView
我们建议你不要使用这个方法，因为与 -viewDidLoad 相比，它没有什么特别的优势，并且有一些缺点。 但是，只要不将 self.view 属性设置为不同的值，它可以安全的使用。 它的 super 方法会将其封装的 UIViewController 的 view 设置为 ASViewController 的 node.view。

#### viewDidLoad
这个方法在 -loadView 之后被执行，这是 ASViewController 生命周期中，你可以访问 node.view 最早的方法，你可以在这份方法中任意修改 view 和 layer 或添加手势，这个方法在其所属的生命周期中，只会执行一次。

所以布局代码不应该放在这个方法中，因为当界面重绘时，这里的代码不会被再次调用。UIViewController 中这个方法也是同样的，在这种方法中放置布局代码是一种不太好的做法，即使你的布局不会因为交互发生变化。

#### viewWillLayoutSubviews
这个方法会与节点的 -layout 同时调用，它可能在 ASViewController 的生命周期中被多次调用，当 ASViewController 的节点的边界发生改变，如旋转、分割屏幕、键盘弹出等行为，或者当视图的层次结构发生变化，如子节点添加、删除或改变大小时，这个方法将被调用。

因为它不经常被调用，但是调用就代表页面需要重绘，因此所有的布局代码最好都放在这个方法中，即使是不直接依赖于 size 的 UI 代码也应放在这里。

#### viewWillAppear / viewDidDisappear
viewWillAppear 在 ASViewController 的节点出现在屏幕上之前被调用，这是节点从屏幕出现的最早时间，viewDidDisappear 在控制器从视图层次结构中移除之后被调用，这是节点从屏幕消失的最早时机，这两个方法提供了一个很好的时机来启动或停止与控制器相关的动画，这也是一个保存和记录用户行为日志的好地方。

尽管这些方法可能被多次调用，并且是可以执行布局代码的，但是这两个方法不会在所有需要重绘的时候被调用，因此除了特定的动画设置之外，不应该用于执行核心的布局代码。



## Node Subclass

| Node Subclass | UIKit Equivalent |
| --- | --- |
| ASDisplayNode | UIView |
| ASCellNode | UITableViewCell / UICollectionViewCell ，可用于容器 ASTableNode、ASCollectionNode和 ASPagerNode |
| ASScrollNode | UIScrollView |
| ASEditableTextNode | UITextView |
| ASTextNode | UILabel |
| ASImageNode | UIImage |
| ASNetworkImageNode | UIImage |
| ASMultiplexImageNode | UIImage |
| ASVideoNode | AVPlayerLayer |
| ASVideoPlayerNode | UIMoviePlayer |
| ASControlNode | UIControl |
| ASButtonNode | UIButton |
| ASMapNode | MKMapView |


### ASDisplayNode

#### init
在使用 initNodeBlocks 时，这个方法会后台线程上被调用。但是，因为没有其他方法会在 init 完成之前运行，所以这个方法不需要加锁。

需要记住的最重要的一点是，init 方法必须能够在任何队列上调用。最值得注意的是，你永远不应该在节点初始化方法中初始化任何 UIKit 对象，以及调用 node.layer node.view.x 等与view 或 layer 有关的操作，也不应该在这个方法中为节点添加手势，这些事件应该在 didLoad 方法中进行。

#### didLoad 
这个方法在概念上类似于 UIViewController 的 -viewDidLoad 方法，当后台视图初始化完成时，它会被调用一次，它保证会在主线程上被调用，是执行任何 UIKit 代码合适的地方，例如添加手势识，更改 view 和 layer，初始化 UIKit 对象。

#### layoutSpecThatFits
该方法定义了节点的布局，并在后台线程上进行了大量的计算。此方法是你声明、创建和修改 ASLayoutSpec 布局描述对象的地方，该对象描述了节点的 size，以及其子节点的 size 和 position，是你放置大部分布局代码的地方。

ASLayoutSpec 对象直到在此方法中返回前是可变的。 在这之后，这个对象将不可改变，需要注意的是你不需要缓存 ASLayoutSpec  对象以备后用，我们建议你在必要时重新创建布局描述。

由于它在后台线程上运行，因此你不能在这个方法中调用 node.view 或 node.layer 以及它们的属性。 此外，除非你明确知道自己在做什么，否则不要在此方法中创建其他节点。 另外，重写此方法并不一定需要调用 super 方法。

#### layout
在此方法中调用 super 将，会使用  layoutSpec 对象计算布局，所有子节点都将计算其 size 和 position。

#### layout
在概念上类似于 UIViewController 的 -viewwilllayoutsubview，这是一个更改 hidden 属性、修改 view 属性、设置背景颜色的好地方。你可以在 -layoutspec: 方法中设定背景颜色，但这可能会存在时序问题。如果你需要使用原生的 UIView，可以在这里设置它们的 frame，不管怎样，你始终可以使用 -initWithViewBlock: 创建节点，并在其他地方的后台线程中进行调整。

这个方法在主线程上被调用，如果你使用的是 ASLayoutSpec，那么你不应该过多地依赖这个方法，因为在主线程上进行布局是非常可取的，需要这个方法的子类小于 1/10。

使用 -layout 的一个重要用途是你需要子节点的 size 是精确的

如果你希望在 ASViewController 中得到相同的效果，那么你可以在 -viewWillLayoutSubviews 中做同样的事情，不过如果你的节点通过 initWithNode: 进行实例化，它会自动做到这一点。


## 布局

+ 布局规则 `ASLayoutSpec`
+ 布局元素 `ASLayoutElement`
+ 布局调试 `Layout Debugging`
 在任何 ASDisplayNode 或 ASLayoutSpec 上调用 -asciiArtString 都会返回该对象及其子项的字符图。 你也可以在任何 Node 或 layoutSpec 中设置 .debugName，这样也将包含字符图

缺少初始固有大小的 Node (ASVideoNode / ASVideoPlayerNode / ASNetworkImageNode / ASEditableTextNode) 必须使用 ASRatioLayoutSpec(比例布局规则)、ASAbsoluteLayoutSpec(绝对布局规则) 或样式对象的 .size 属性为它们设置初始大小。

| 规则| 描述 |
| --- | --- |
| ASWrapperLayoutSpec| 填充布局 |
| ASStackLayoutSpec| 盒子布局 |
| ASInsetLayoutSpec| 插入布局 |
| ASOverlayLayoutSpec| 覆盖布局 |
| ASBackgroundLayoutSpec| 背景布局 |
| ASCenterLayoutSpec| 中心布局 |
| ASRatioLayoutSpec| 比例布局 |
| ASRelativeLayoutSpec| 顶点布局 |
| ASAbsoluteLayoutSpec| 绝对布局 |


### ASStackLayoutSpec

direction：主轴的方向，有两个可选值：

+ 纵向：ASStackLayoutDirectionVertical
+ 横向：ASStackLayoutDirectionHorizontal

spacing: 主轴上视图排列的间距，比如有四个视图，那么它们之间的存在三个间距值都应该是spacing

justifyContent: 主轴上的排列方式，有五个可选值：

+ ASStackLayoutJustifyContentStart 从前往后排列
+ ASStackLayoutJustifyContentCenter 居中排列
+ ASStackLayoutJustifyContentEnd 从后往前排列
+ ASStackLayoutJustifyContentSpaceBetween 间隔排列，两端无间隔
+ ASStackLayoutJustifyContentSpaceAround 间隔排列，两端有间隔

alignItems: 交叉轴上的排列方式，有五个可选值：

+ ASStackLayoutAlignItemsStart 从前往后排列
+ ASStackLayoutAlignItemsEnd 从后往前排列
+ ASStackLayoutAlignItemsCenter 居中排列
+ ASStackLayoutAlignItemsStretch 拉伸排列
+ ASStackLayoutAlignItemsBaselineFirst 以第一个文字元素基线排列（主轴是横向才可用）
+ ASStackLayoutAlignItemsBaselineLast 以最后一个文字元素基线排列（主轴是横向才可用）

children: 包含的视图。数组内元素顺序同样代表着布局时排列的顺序，所以需要注意


主轴的方向设置尤为重要，如果主轴设置的是 ASStackLayoutDirectionVertical, 那么 justifyContent 各个参数的意义就是：

+ ASStackLayoutJustifyContentStart 从上往下排列
+ ASStackLayoutJustifyContentCenter 居中排列
+ ASStackLayoutJustifyContentEnd 从下往上排列
+ ASStackLayoutJustifyContentSpaceBetween 间隔排列，两端无间隔
+ ASStackLayoutJustifyContentSpaceAround 间隔排列，两端有间隔

alignItems 就是：

+ ASStackLayoutAlignItemsStart 从左往右排列
+ ASStackLayoutAlignItemsEnd 从右往左排列
+ ASStackLayoutAlignItemsCenter 居中排列
+ ASStackLayoutAlignItemsStretch 拉伸排列
+ ASStackLayoutAlignItemsBaselineFirst 无效
+ ASStackLayoutAlignItemsBaselineLast 无效




**未完待续...**

### 参考资料

+ http://texturegroup.org/docs/node-overview.html
+ https://juejin.im/post/5a16acf56fb9a04509092ce5
+ https://juejin.im/post/5a1be41351882561a20a32e9
+ https://segmentfault.com/a/1190000007991853#articleHeader7
+ https://bawn.github.io/2017/12/Texture-Layout/

### 相关阅读

+ https://draveness.me/layout-performance
