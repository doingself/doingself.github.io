---
title: "什么是 EXC_BAD_ACCESS 以及如何调试它"
date: 2018-09-06 22:33:44
categories: 
	- IOS
tags:
	- Crashed
description: "EXC_BAD_ACCESS KERN_INVALID_ADDRESS"
copyright: true
---

原文链接: https://code.tutsplus.com/tutorials/what-is-exc_bad_access-and-how-to-debug-it--cms-24544

由 [巴特雅各布斯](https://tutsplus.com/authors/bart-jacobs?_ga=2.112966724.1797758793.1557374170-2093790351.1557374170) 2015年8月14日 发布于 [envatotuts+](https://code.tutsplus.com/tutorials/what-is-exc_bad_access-and-how-to-debug-it--cms-24544)

在某个时刻，您将遇到由EXC_BAD_ACCESS引起的崩溃。在这个快速提示中，您将了解EXC_BAD_ACCESS是什么以及它是由什么引起的。我还将提供一些技巧来修复由EXC_BAD_ACCESS引起的错误。

# 什么是EXC_BAD_ACCESS？

一旦你理解了EXC_BAD_ACCESS的根本原因，你就会更好地理解它的神秘名称。有一个简单的解释和更多的技术解释。让我们先从简单的解释开始。

### 保持简单

每当遇到EXC_BAD_ACCESS时，就意味着您正在向已经释放的对象发送消息。这是最常见的情况，但有一些例外，我们稍后会讨论。

### 这真的意味着什么

技术解释有点复杂。在C和Objective-C中，你经常处理 指针。指针只不过是一个存储另一个变量的内存地址的变量。向对象发送消息时，指向要发送消息的对象的指针需要取消引用。这意味着您获取指针指向的内存地址并访问该内存块的值。

当不再为您的应用程序映射该内存块时，换句话说，该内存块不会用于您认为使用的内存，则不再可能访问该内存块。发生这种情况时，内核会发送一个异常（EXC），表明您的应用程序无法访问该内存块（BAD ACCESS）。

总之，当您遇到EXC_BAD_ACCESS时，这意味着您尝试将消息发送到无法执行该消息的内存块。

但是，在某些情况下，EXC_BAD_ACCESS是由损坏的指针引起的。每当您的应用程序尝试取消引用损坏的指针时，内核都会抛出异常。

# 调试EXC_BAD_ACCESS
调试EXC_BAD_ACCESS可能会非常棘手且令人沮丧。但是，现在EXC_BAD_ACCESS不再是你的谜，它应该不那么令人生畏。

您需要了解的第一件事是，当您的应用程序无法再访问内存块时，您的应用程序不一定会崩溃。这就是经常调试EXC_BAD_ACCESS这么困难的原因。

腐败指针也是如此。您的应用程序不会崩溃，因为指针已损坏。如果在应用程序中传递损坏的指针，它也不会崩溃。但是，当您的应用程序尝试取消引用损坏的指针时，就会出错。

### Zombies (植物大战僵尸)
虽然僵尸在过去几年中越来越受欢迎，但它们已经在Xcode中存在了十多年。名称  zombie可能听起来有点戏剧性，但它实际上是一个很好的名称，可以帮助我们调试EXC_BAD_ACCESS。让我解释它是如何工作的。

在Xcode中，您可以启用僵尸对象，这意味着解除分配的对象会像僵尸一样被保留。换句话说，释放的对象保持活动以进行调试。没有魔法涉及。如果您向僵尸对象发送消息，则您的应用程序仍将因EXC_BAD_ACCESS而崩溃。

为什么这有用？EXC_BAD_ACCESS难以调试的原因是您不知道应用程序尝试访问的对象。僵尸对象在很多情况下解决了这个问题。通过保持释放的对象存活，Xcode可以告诉您尝试访问的对象，使搜索问题变得更加容易。

在Xcode中启用僵尸非常容易。请注意，这可能会因您使用的Xcode版本而异。以下方法适用于Xcode 6和7.单击左上角的活动方案，然后选择“  编辑方案”。

选择   左侧的“运行 ”，然后打开顶部的“  诊断” 选项卡。要启用僵尸对象，请勾选标记为Enable Zombie Objects的复选框  。

在当前方案中启用僵尸对象
如果您现在遇到EXC_BAD_ACCESS，Xcode控制台中的输出将让您更好地了解从何处开始搜索。看一下下面的示例输出。

> 2015-08-12 06:31:55.501 Debug[2371:1379247] -[ChildViewController respondsToSelector:] message sent to deallocated instance 0x17579780

在上面的例子中，Xcode告诉我们 `respondsToSelector:` 发送给僵尸对象的消息。但是，僵尸对象不再是 `ChildViewController` 该类的实例  。之前分配给 `ChildViewController` 实例的内存块不再映射到您的应用程序。这应该可以让您很好地了解问题的根本原因。

不幸的是，僵尸对象将无法为EXC_BAD_ACCESS导致的每次崩溃节省一天的时间。如果僵尸对象没有做到这一点，那么就该进行适当的分析。

### 分析
如果僵尸对象无法解决您的问题，那么根本原因可能不那么重要。在这种情况下，您需要仔细查看应用程序崩溃时正在执行的代码。这可能是麻烦且耗时的。

为了帮助您找到代码库中的问题，您可以让Xcode分析您的代码，以帮助您找到有问题的区域。请注意，Xcode会分析您的项目，这意味着它会指出遇到的每个潜在问题。

要告诉Xcode分析您的项目，请从Xcode的“  产品”菜单中选择“  分析”或按  Shift-Command-B。Xcode需要一些时间，但是当它完成时，您应该在左侧的Issue Navigator中看到一个问题列表  。分析中发现的问题以蓝色突出显示。

问题以蓝色突出显示
当您单击某个问题时，Xcode会将您带到需要您注意的代码块。请注意，Xcode只是提出建议。在某些情况下，问题可能不相关且不需要修复。

Xcode会告诉你代码究竟出了什么问题
如果您找不到导致EXC_BAD_ACCESS的错误，那么仔细检查Xcode在项目分析过程中发现的每个问题都很重要。

# 结论
EXC_BAD_ACCESS是开发人员常见的挫败感，它是手动内存管理所固有的。自引入ARC（自动引用计数）以来，与内存管理相关的问题变得越来越少，但它们绝不会消失。

原文链接: https://code.tutsplus.com/tutorials/what-is-exc_bad_access-and-how-to-debug-it--cms-24544
