---
title: 理解sync.once
description: sync.once的源码理解和拓展
date: 2025-01-05 10:00:00+0800
categories: ["编程", "golang"]
tags: ["golang"]
weight: 2
---

### sync.once的使用

`sync.Once`是go标准库的一个类型，用于在并发环境中保证某一段代码只被执行一次。 

通常，我们会用它来进行一些初始化的工作。比如:

```go
func Test_Once(t *testing.T) {
    var once sync.Once
    done := make(chan bool)
    for i := 0; i < 10; i++ {
        go func() {
            once.Do(onceFunc) // 执行十次
            done <- true
        }()
    }
    for i := 0; i < 10; i++ {
        <-done
    }
    fmt.Println(KeyCount) // output: 1
}

var KeyCount int

func onceFunc() {
    KeyCount++
}

```

上面的例子中，onceFunc会被执行10次，但是因为我们用了`sync.Once`，所以最终KeyCount只会被加一次。


### sync.once的源码
所以，他是怎么实现的呢？ 我们在IDE中点开`sync.Once`的实现。


``` go
type Once struct {
    done uint32
    m    Mutex
}
```

结构很简单，done用来标记执行过的状态，m就是锁。说白了还是靠锁。
继续看：

``` go
func (o *Once) Do(f func()) {
    if atomic.LoadUint32(&o.done) == 0 {
        o.doSlow(f)
    }
}

func (o *Once) doSlow(f func()) {
    o.m.Lock()
    defer o.m.Unlock()
    if o.done == 0 {
        defer atomic.StoreUint32(&o.done, 1)
        f()
    }
}
```

这里就是比较有趣的地方了，

首先他通过`atomic.LoadUint32`这个原子操作来判断done这个Int是否已经被改过了。  
并发操作有个原则，能用`原子操作`的地方别用锁。原子操作效率更高。  
在`doSlow`中，可以看到加锁操作，临界区里再进行一次状态判断，随后改掉状态值。

- 问题：明明已经拿到锁了，为什么还要再执行一次`if o.done == 0`的判断呢?

仔细考虑了下，猜测在并发环境下，两个协程有机会同时通过了`atomic.LoadUint32(&o.done) == 0`的检查，他们先后调用doSlow  
比如协程A先拿到锁进入临界区，然后释放锁。 此时协程B会把临界区代码再执行一次！   
因此临界区里也需要加入`if o.done == 0`来让协程B早点返回。

sync.once的代码写得真好啊，又短又精，很适合让我们学习。

### sync.once的扩展

sync.Once可以保证代码只被执行一次，看完源码我的脑洞就来了。  
工作中有一些执行比较重的后端接口（比如计划任务）需要有调用频率保护，  
前端不小心连续点击了5次，而后端只希望5次中的1次可以成功调用，在这个接口执行完毕并返回以前，其他点击都要废弃。  

相当于保证同时只有一个实例在跑。是不是可以借用这个Once的实现呢？

然后我就写一版。

``` golang
type LimitOne struct {
    done uint32
    m    sync.Mutex
}

var LimitError = fmt.Errorf("We are working on the task! please try again later")

func (o *LimitOne) Do(f func() error) error {
    if atomic.LoadUint32(&o.done) == 1 {
        return LimitError
    }
    o.m.Lock()
    defer o.m.Unlock()

    if o.done == 0 {
        atomic.StoreUint32(&o.done, 1)
        defer atomic.StoreUint32(&o.done, 0)
        err := f()
        return err
    }
    return LimitError
}
```

大体按照sync.once的结构抄一遍， 唯一不同的是当传入的函数执行完之后，需要把done再设置为0。  
此时又处于就绪状态，又可以接新的请求了。  
以此来保证不论有多少请求，`保证每次只有一个task在跑`。

下班~