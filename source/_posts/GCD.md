---
title: "GCD"
date: 2018-10-26 18:33:44
categories:
    - IOS
    - Swift
tags:
    - GCD
description: ""
copyright: true
---

# GCD

```
func groupTask(name: String, completed: @escaping (_ name: String)->Void){
    
    // 模拟延迟执行
    // 随机数(1...10)
    let random1 = Int(arc4random()%10)+1
    let random2 = Int(arc4random_uniform(10))+1
    
    DispatchQueue.global().async {
        print("DispatchQueue.global().async \t \(name) , random=\(random1) , random=\(random2)")
        sleep(UInt32(random2))
        DispatchQueue.main.async {
            print("DispatchQueue.main.async \t \(name) , random=\(random1) , random=\(random2)")
            completed(name)
        }
    }
}
```

# DispatchGroup

```
// DispatchGroup用来管理一组任务的执行，然后监听任务都完成的事件。比如，多个网络请求同时发出去，等网络请求都完成后reload UI。
// enter/leave 成对出现, 最终执行 notify
let group = DispatchGroup()

for i in 0...10{
    group.enter()
    groupTask(name: "name=group\(i)") { (name) in
        print("completed \(name)")
        group.leave()
    }
}

group.notify(queue: DispatchQueue.main) {
    print("DispatchGroup notify DispatchQueue.main")
}
```

# DispatchSemaphore

```
// DispatchSemaphore是传统计数信号量的封装，用来控制资源被多任务访问的情况。
let sema = DispatchSemaphore(value: 2)
let queue = DispatchQueue(label: "semaphore-test")

for i in 0...10{
    queue.async {
        sema.wait()
        groupTask(name: "name=sema\(i)") { (name) in
            print("completed \(name)")
            sema.signal()
        }
    }
}
```

# barrier

```
// barrier翻译过来就是屏障。在一个并行queue里，很多时候，我们提交一个新的任务需要这样做。
// queue里之前的任务执行完了新任务才开始
// 新任务开始后提交的任务都要等待新任务执行完毕才能继续执行
// 以barrier flag提交的任务能够保证其在并行队列执行的时候，是唯一的一个任务。（只对自己创建的队列有效，对gloablQueue无效
// ...
```

# 鸣谢

+ https://blog.csdn.net/Hello_Hwc/article/details/54293280
