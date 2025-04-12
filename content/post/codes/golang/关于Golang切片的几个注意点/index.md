---
title: 关于Golang切片的几个注意点
description: Golang切片
date: 2025-01-02 08:00:00+0800
categories: ["编程", "golang"]
tags: ["golang"]
weight: 4
---

今天刷题`leetcode no.78`-子集问题，其中遇到一个Slice的语法坑，  
查了半小时最后才定位是切片的使用问题。  
搞定之后顺便翻阅了一下slice的实现原理，把以前总结的点也放一起做个小记录。


### slice的实现
先看一下slice的结构体(https://go.dev/src/runtime/slice.go)定义：

``` go
type slice struct {
	array unsafe.Pointer
	len   int
	cap   int
}
```

一个切片本身指向了一个底层数组，  
len表示目前的元素数量，cap表示这个slice最大的元素容量

### 注意区分len和cap
len和cap是第一个要理清的概念，特别是当我们使用make去创建新切片时

``` go
s1 := make([]int, 3) 
s2 := make([]int, 0, 3)
fmt.Println(s1, s2) 
```

在上面的代码中，s1的输出结果是[0 0 0]，
cap=len=3,有3个元素，最大容量是3，3个元素都是这个类型的初始值，也就是0

而s2的结果是[]，cap=3 len=0, 目前是0个元素，是个空切片。

### 扩容的性能问题
容量 = cap, 是目前切片已经预分配的内存能够容纳的最大元素个数

我们通过append操作向切片加元素，一但超过了切片的容量，就需要分配新的内存，并将当前切片所有的元素拷贝到新的内存块上。

这里就有一道喜闻乐见的面试题了，

> go的切片扩容机制是什么样的


而这个答案直接看源码就能找到，注释是这么写的：

``` go
// Transition from growing 2x for small slices
// to growing 1.25x for large slices. This formula
```

切片在容量在比较小的时候，容量是通过x2的倍数扩大的，
cap当达到一个阈值时，以1.25倍的方式扩容。

所以我们写代码的时候要注意两点：  
- 尽量在声明时就确定切片的大小，免得反复扩容。  
- 大容量的切片应该复用，减少频繁申请内存。  

### Slice的切片操作不会生成新的数组  

这个问题就是我今天遇到的，当我们对一个切片执行切片操作时，新的切片指向的是原来切片的底层数组。  
所以当我们向新的切片append的时候，也改变了老切片的值

示例代码：
``` go
func Test_Slice(t *testing.T) {
	s1 := []int{1, 2, 3, 4}
	s2 := s1[:2]  // 切片操作 s2为[1 2]

	s2 = append(s2, 5)  //  s2为[1 2 5]
	fmt.Println(s1) // 现在s1是多少呢?
}
```

最后s1的值是 [1 2 5 4]!! 很难理解吧。  
因为上面的代码里，s2是从s1切片出来的，向s2添加元素后，实际上也在修改s1的值

想要生成一个完全不同的底层数组怎么办？

引申：拷贝一个切片的最佳实践

``` go
func CopySlice() {
	nums := []int{1, 2, 3}
	newNums := make([]int, len(nums))
	copy(newNums, nums)
}
```
