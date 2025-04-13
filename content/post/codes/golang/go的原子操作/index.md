---
title: go的原子操作
description: go的原子操作
date: 2025-03-09 08:00:00+0800
categories: ["编程", "golang"]
tags: ["golang"]
weight: 4
---

想要实现一个无锁的并发程序编写，那么直接对变量进行原子操作就是很好的选择，趁这个机会把go提供的几个原子方法学习一下。

go在`sync/atomic`包里对Int32，Int64等几个基本类型都定义了相同的一套方法， 下面以Int64为例：

几个方法的含义比较简单，从函数名就能看出含义。

>StoreInt64 存  
LoadInt64 取  
AddInt64 加法  
SwapInt64 交换  
CompareAndSwapInt64 判断并交换

``` go
func Test_AtomicFunc(t *testing.T) {
    var counter int64 = 0
    atomic.StoreInt64(&counter, 10)
    fmt.Println(counter) // output: 10
    res := atomic.LoadInt64(&counter)
    fmt.Println(res) // output: 10
    atomic.AddInt64(&counter, 2)
    fmt.Println(counter) // output: 12
    atomic.SwapInt64(&counter, 22)
    fmt.Println(counter) // output: 22
    atomic.CompareAndSwapInt64(&counter, 22, 32)
    fmt.Println(counter) // output: 32
}
```


我们平时用地比较多的是`CompareAndSwap`（简称 CAS）方法，假如值不等于old，那就设置成一个新值new.
这可以在不加锁的前提下完成对变量值的判断和更新

``` go

func Test_CompareAndSwap(t *testing.T) {
    var first int64 = 0

    for i := 1; i <= 1000; i++ {
        go func(i int) {
            if atomic.CompareAndSwapInt64(&first, 0, int64(i)) {
                log.Println("第一个完成赋值的goroutine", i)
            }
        }(i)
    }

    time.Sleep(2 * time.Second)
    log.Println("最后的值 num:", atomic.LoadInt64(&first))
}


// 输出结果:

=== RUN   Test_CompareAndSwap
2022/05/07 00:11:18 第一个完成赋值的goroutine 13
2022/05/07 00:11:20 最后的值 num: 13
``` 

我们可以看出虽然开了1000个goroutine,但是只有第13个抢到了赋值的机会.

在他赋值之后，其他的goroutine都会compare失败而退出，从而不需要加锁就完成了竞争条件的设定。

简单好用~