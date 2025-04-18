---
title: 位运算表示int的范围
description: 位运算表示int的范围
date: 2023-07-29 08:00:00+0800
categories: ["编程"]
tags: []
weight: 4
---

### int32的取值范围

今天刷题时需要用到int32的取值范围，查了下是“-2147483648”到“2147483647”，有没有办法快速表示这个数呢？

在我翻了其他人的leetcode提交后发现一个有趣的代码：

```
const ( 
	int32min = -1 << 31 
	int32max = 1<<31 - 1
)

// 写个UT打印一下, 也是正确的。
func Test_PrintConstInt32(t *testing.T) {
	fmt.Println(int32min, int32max) // -2147483648 2147483647
}
```

关于位运算很多东西都扔回给老师了，今天再补一下知识

### 位运算，左移和右移
- << 表示左移
对于一个正数 左移N位 = 乘2n次方   
比如：  
1100（12） 左移1位 –> 11000(24)

- \>> 表示右移
对于一个正数 右移N位 = 除2n次方  
比如：  
1100（12） 右移1位 –> 110(6)


int32的范围大小: `[-1 << 31，1<<31 - 1]`

精准，高逼格，High Level!