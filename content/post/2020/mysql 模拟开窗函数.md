---
title: "mysql 子查询模拟开窗函数"
date: 2020-03-25T18:53:20+08:00
Description: "今天写一个SQL时需要使用开窗函数"
Tags: [mysql,sql]
Categories: []
DisableComments: false
---


今天写一个SQL时需要使用开窗函数，结果发现Mysql在8.0后才开始支持CTE和开窗函数，  
我们目前所使用的版本比较低，所以我想了一种通过子查询来模拟的方法。

### 举个例子：  

现在有一张普通的表 `table_age`，只有三列：ID, name, age.

![原表](/img/2020/1.webp)

现在想让每一行都带上年龄的和，类似开窗函数的效果


![结果](/img/2020/2.webp)

可以像下面这样写：
```
select 
	tempT.totalAge as '总年龄',
	table_age.*
from 
	table_age,
	(select sum(age) as "totalAge" from table_age) tempT
```

其他开窗函数可以用这样类似的方法去模拟。