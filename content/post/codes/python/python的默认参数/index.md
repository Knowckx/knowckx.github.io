+++
title = "Python函数的默认参数为何不能是[]或{}？"
slug = "python-mutable-default-arguments-pitfall"
description = "今天在写一个接口爬虫时，遇到了一个Python中关于函数默认参数的经典陷阱..."
categories = ["python"]
tags = ["python","最佳实践"]
keywords = ["python", "函数", "默认参数", "可变对象"]
weight = 4
date = "2025-07-24 11:12:34+0800"
+++


今天在写一个股票行情的接口爬虫，为了方便我写了一个发送 POST 请求的函数。  
签名类似于 `do_post(url, post_data={})`  
然后我把全部代码扔给`Gemini`进行检查优化，它竟然提示我 `post_data={}`这个写法存在一个经典的陷阱  
我半信半疑去写了个简单的测试，结果发现还真是这样。  

我觉得很多人和我一样，也不知道这一点，过来记录一下


### 复现
代码真的很简单，你可以先预测一下它的输出是什么。

```python
def fn_test(input_map={}):
	print(f"函数开始时, input_map 是: {input_map}")
	input_map["token"] = 123
	print(f"函数结束时, input_map 变成: {input_map}\n")

print("第一次调用")
fn_test()

print("第二次调用")
fn_test()
```

---


如果你认为两次调用的输出会一模一样，那就掉进陷阱里了。

实际的输出结果是：

```
--- 第一次调用 ---
函数开始时, input_map 是: {}
函数结束时, input_map 变成: {'token': 123}

--- 第二次调用 ---
函数开始时, input_map 是: {'token': 123}
函数结束时, input_map 变成: {'token': 123}
```

第一次调用的输出是正常的


但是第二次调用的输出就不对了，为什么第二次调用的第一次打印会是`{'token': 123}` ？ 默认值不应该是一个空字典`{}`吗？

### 原因
这里python为我们留了一个坑，当函数声明的默认参数是一个可变对象时，比如dict或者list类型，

所有外面在调用此函数未提供具体参数的，他们实际上`都会共享同一个对象`。

也就是第一次无参调用此函数，和第二次无参调用此函数，他们指向了同一个dict.

那么假如在某次调用过程中，**无意间修改了这个dict**，这个修改就会**污染**后续所有无参的调用过程

### 解决方法:

把签名改成

```python
def fn_test(inputMap:dict|None = None):
    pass
```

只声明他可能会属于dict这个类型，但是实际值默认是`None`

反正记住一个简单的规则：不要使用`可变对象（如 [] 或 {}）`作为函数的默认参数就可以了。

> AI写业务代码真的太强了，我准备好退休了





