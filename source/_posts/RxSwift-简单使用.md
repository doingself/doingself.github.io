---
title: "RxSwift 简单使用"
date: 2018-03-29 15:03:04
categories:
	- IOS
	- Swift
tags:
	- RxSwift
description: 
copyright: true
---

# RxSwiftDemo
详情参看我的 [RxSwiftDemo](https://github.com/doingself/RxSwiftDemo)

# RxSwift

[Rx](http://reactivex.io) 是 ReactiveX 的缩写，简单来说就是基于异步 Event（事件）序列的响应式编程。
Rx 可以简化异步编程方法，并提供更优雅的数据绑定。让我们可以时刻响应新的数据同时顺序地处理它们。

## Observable

`Observable<T>` 这个类就是 Rx 框架的基础，我们可以称它为可观察序列。它的作用就是可以异步地产生一系列的 Event（事件），即一个 Observable<T> 对象会随着时间推移不定期地发出 event(element : T) 这样一个东西。
还需要有一个 Observer（订阅者）来订阅 Observable<T> 不时发出的 Event。

### 初始化 Observable
http://www.hangge.com/blog/cache/detail_1922.html

+ `Observable<Int>.empty()` 创建一个空内容的 Observable 序列
+ `Observable<Int>.just(5)` 通过传入一个默认值来初始化
+ `Observable.of("A", "B", "C")` 接受可变数量的参数（必需要是同类型的）
+ `Observable.from(["A", "B", "C"])` 同 `of`
+ `Observable.range(start: 1, count: 5)` == `Observable.of(1,2,3,4,5)`
+ `Observable.generate(initialState: 0, condition: { $0 <= 10 }, iterate: { $0 + 2 })` == `of(0,2,4,6,8,10)`
+ `Observable.repeatElement(1)` 一个可以无限发出给定元素的 Event 的 Observable 序列（永不终止）。 
+ `Observable<Int>.never()` 一个永远不会发出 Event（也不会终止）的 Observable 序列
+ `Observable<Int>.error()` 一个不做任何操作，而是直接发送一个错误的 Observable 序列。
+ `create` 和 `deferred` 参数为 block
+ `Observable<Int>.interval(1, scheduler: MainScheduler.instance)` 每1秒发送一次，并且是在主线程（MainScheduler）发送。interval创建的 Observable 序列每隔一段设定的时间，会发出一个索引数的元素。而且它会一直发送下去。
+ `Observable<Int>.timer(5, scheduler: MainScheduler.instance)` 延时5秒种后, 发出唯一的一个元素0
+ `Observable<Int>.timer(5, period: 2, scheduler: MainScheduler.instance)` 延时5秒种后，每隔2秒钟发出一个元素

### 订阅 Observable

+ `subscribe(_ on: @escaping (RxSwift.Event<Self.E>) -> Swift.Void) -> Disposable` 订阅了一个 Observable 对象，该方法的 block 的回调参数就是被发出的 event 事件
+ `subscribe(onNext: ((Self.E) -> Swift.Void)? = default, onError: ((Error) -> Swift.Void)? = default, onCompleted: (() -> Swift.Void)? = default, onDisposed: (() -> Swift.Void)? = default) -> Disposable` 可以只处理 onNext

### doOn 监听生命周期

一个 Observable 序列被创建出来后它不会马上就开始被激活从而发出 Event，而是要等到它被某个人订阅了才会激活它。激活之后要一直等到它发出了 .error 或者 .completed 的 event 后，它才被终结。

doOn 方法来监听事件的生命周期，它会在每一次事件发送前被调用。参数同subscribe

```
let obs = Observable.of(1,2,3)
        
obs.do(onNext: { (a) in
	    print("do onNext \(a)")
	}, onError: { (e) in
	    
	}, onCompleted: {
	    print("do onCompleted")
	}, onSubscribe: {
	    print("do onSubscribe")
	}, onSubscribed: {
	    print("do onSubscribed")
	}, onDispose: {
	    print("do onDispose")
	})
	.subscribe(onNext: { (element: Int) in
	    print("subscribe onNext \(element)")
	}, onError: { (err: Error) in
	    print(err)
	}, onCompleted: {
	    print("subscribe onCompleted")
	}, onDisposed: {
	    print("subscribe onDisposed")
	})
```

```
do onSubscribe
do onSubscribed
do onNext 1
subscribe onNext 1
do onNext 2
subscribe onNext 2
do onNext 3
subscribe onNext 3
do onCompleted
subscribe onCompleted
subscribe onDisposed
do onDispose
```

### Observable 的销毁

+ `sub.disposed(by: DisposeBag())` DisposeBag 来管理多个订阅行为的销毁
+ `sub.dispose()` 方法把这个订阅给销毁掉

## Observer 观察者

观察者（Observer）的作用就是监听事件，然后对这个事件做出响应。

### 创建观察者

http://www.hangge.com/blog/cache/detail_1941.html

+ `subscribe` + block
+ `bind` + block
+ `AnyObserver` + subscribe / bindTo, AnyObserver 可以用来描叙任意一种观察者。
+ `Binder` + subscribe / bindTo, Binder是在给定 Scheduler 上执行（默认 MainScheduler）不会处理错误事件 `Binder(label){(lab, val) in ... }`

### 自定义可绑定属性

```
import UIKit
import RxSwift
import RxCocoa

// 对 Reactive 进行扩展
extension Reactive where Base: UILabel {
	// 给 UILabel 增加了一个 fontSize 可绑定属性。
    public var fontSize: Binder<CGFloat> {
        return Binder(self.base) { lab, val in
            lab.font = UIFont.systemFont(ofSize: val)
        }
    }
}

//绑定属性
.bind(to: label.rx.fontSize)
```

## Subjects

http://www.hangge.com/blog/cache/detail_1929.html

Subjects 既是订阅者，也是 Observable：能够动态地接收新的值，有了新的值之后，就会通过 Event 将新值发出给他的所有订阅者。

### PublishSubject

`let subject = PublishSubject<String>()`

PublishSubject 是最普通的 Subject，它不需要初始值就能创建。
PublishSubject 的订阅者从他们开始订阅的时间点起，可以收到订阅后 Subject 发出的新 Event，而不会收到他们在订阅前已发出的 Event。


### BehaviorSubject

`let subject = BehaviorSubject(value: "111")`

BehaviorSubject 需要通过一个默认初始值来创建。
当一个订阅者来订阅它的时候，这个订阅者会立即收到 BehaviorSubjects 上一个发出的 event。之后就跟正常的情况一样，它也会接收到 BehaviorSubject 之后发出的新的 event。

### ReplaySubject

`let subject = ReplaySubject<String>.create(bufferSize: 2)`

ReplaySubject 在创建时候需要设置一个 bufferSize，表示它对于它发送过的 event 的缓存个数。subscriber 订阅了这个 ReplaySubject，那么这个 subscriber 就会立即收到前面缓存的 .next 的 event。
如果一个 subscriber 订阅已经结束的 ReplaySubject，除了会收到缓存的 .next 的 event 外，还会收到那个终结的 .error 或者 .complete 的 event。

### Variable

`let variable = Variable("111")`

Variable 其实就是对 BehaviorSubject 的封装，所以它也必须要通过一个默认的初始值进行创建。
Variable 具有 BehaviorSubject 的功能，能够向它的订阅者发出上一个 event 以及之后新创建的 event。

Variable 还把会把当前发出的值保存为自己的状态。同时它会在销毁时自动发送 .complete 的 event，不需要也不能手动给 Variables 发送 completed 或者 error 事件来结束它。
简单地说就是 Variable 有一个 value 属性，我们改变这个 value 属性的值就相当于调用一般 Subjects 的 onNext() 方法，而这个最新的 onNext() 的值就被保存在 value 属性里了，直到我们再次修改它。

Variables 本身没有 subscribe() 方法，但是所有 Subjects 都有一个 asObservable() 方法。我们可以使用这个方法返回这个 Variable 的 Observable 类型，拿到这个 Observable 类型我们就能订阅它了。


## 操作符

### 变换操作
http://www.hangge.com/blog/cache/detail_1932.html

变换操作指的是对原始的 Observable 序列进行一些转换，类似于 Swift 中 CollectionType 的各种转换。

+ `buffer` 缓冲组合，第一个参数是缓冲时间，第二个参数是缓冲个数，第三个参数是线程。
+ `window` 实时发出元素序列
+ `map` 把原来的 Observable 序列转变为一个新的 Observable 序列
+ `flatMap` 降维成一个 Observable 序列
+ `flatMapLatest` 与 flatMap 的唯一区别是：flatMapLatest 只会接收最新的 value 事件
+ concatMap
+ scan
+ groupBy

### 过滤操作
http://www.hangge.com/blog/cache/detail_1933.html

+ `filter` 过滤
+ `distinctUntilChanged` 过滤掉连续重复的事件
+ `single` 
+ `elementAt` 只处理指定位置的事件
+ `ignoreElements` 忽略掉所有的元素，只发出 error 或 completed 事件
+ `take` 仅发送 Observable 序列中的前 n 个事件，在满足数量之后会自动 .completed
+ `takeLast` 仅发送 Observable 序列中的后 n 个事件
+ `skip` 跳过源 Observable 序列发出的前 n 个事件。
+ `sample`
+ `debounce` 过滤掉高频产生的元素, 常用在用户输入的时候，不需要每个字母敲进去都发送一个事件，而是稍等一下取最后一个事件。

### 条件操作
http://www.hangge.com/blog/cache/detail_1948.html

+ `amb`
+ `takeWhile`
+ `takeUntil`
+ `skipWhile`
+ `skipUntil`

### 结合操作
http://www.hangge.com/blog/cache/detail_1930.html

+ `startWith`
+ `merge`
+ `zip`
+ `combineLatest`
+ `withLatestFrom`
+ `switchLatest`

### 算数、以及聚合操作
http://www.hangge.com/blog/cache/detail_1934.html

+ `toArray`
+ `reduce`
+ `concat`

### 连接操作
http://www.hangge.com/blog/cache/detail_1935.html

+ publish
+ replay
+ multicast
+ refCount
+ share

### 其他操作
http://www.hangge.com/blog/cache/detail_1950.html

+ delay
+ delaySubscription
+ materialize
+ dematerialize
+ timeout
+ using

### 错误处理操作
http://www.hangge.com/blog/cache/detail_1936.html

+ catchErrorJustReturn
+ catchError
+ retry

### 调试操作
http://www.hangge.com/blog/cache/detail_1937.html

+ debug
+ RxSwift.Resources.total

## 特征序列 Traits

http://www.hangge.com/blog/cache/detail_1939.html
+ Single
+ Completable
+ Maybe

http://www.hangge.com/blog/cache/detail_1942.html
+ Driver

http://www.hangge.com/blog/cache/detail_1943.html
+ ControlProperty
+ ControlEvent

## 调度器 Schedulers
http://www.hangge.com/blog/cache/detail_1940.html

调度器（Schedulers）是 RxSwift 实现多线程的核心模块，它主要用于控制任务在哪个线程或队列运行。

+ CurrentThreadScheduler：表示当前线程 Scheduler。（默认使用这个）
+ MainScheduler：表示主线程。如果我们需要执行一些和 UI 相关的任务，就需要切换到该 Scheduler 运行。
+ SerialDispatchQueueScheduler：封装了 GCD 的串行队列。如果我们需要执行一些串行任务，可以切换到这个 Scheduler 运行。
+ ConcurrentDispatchQueueScheduler：封装了 GCD 的并行队列。如果我们需要执行一些并发任务，可以切换到这个 Scheduler 运行。
+ OperationQueueScheduler：封装了 NSOperationQueue。


## UI
+ label http://www.hangge.com/blog/cache/detail_1963.html
+ textfield textview http://www.hangge.com/blog/cache/detail_1964.html
+ button http://www.hangge.com/blog/cache/detail_1969.html
+ switch segmentController http://www.hangge.com/blog/cache/detail_1970.html
+ activityIndicatorView http://www.hangge.com/blog/cache/detail_1971.html
+ slider stepper http://www.hangge.com/blog/cache/detail_1972.html
+ 双向绑定 http://www.hangge.com/blog/cache/detail_1974.html
+ UIGestureRecognizer http://www.hangge.com/blog/cache/detail_1975.html
+ datePicker http://www.hangge.com/blog/cache/detail_1973.html
+ tableView
	+ 简单的显示 http://www.hangge.com/blog/cache/detail_1976.html
	+ [RxDataSources](https://github.com/RxSwiftCommunity/RxDataSources) 第三方库 是使用 RxSwift 对 UITableView 和 UICollectionView 的数据源做了一层包装 http://www.hangge.com/blog/cache/detail_1982.html
	+ 数据刷新 http://www.hangge.com/blog/cache/detail_1987.html
	+ 搜索 http://www.hangge.com/blog/cache/detail_1994.html
	+ 单元格编辑 http://www.hangge.com/blog/cache/detail_1995.html
	+ 多类型单元格 http://www.hangge.com/blog/cache/detail_1996.html
+ collectionView http://www.hangge.com/blog/cache/detail_2004.html

# TODO

---

鸣谢

+ 航歌 - 做最好的开发者知识平台
+ http://www.hangge.com/blog/cache/detail_1917.html
