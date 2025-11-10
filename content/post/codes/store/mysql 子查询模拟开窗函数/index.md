---
title: mysql模拟开窗函数
description: mysql使用子查询来模拟开窗函数
date: 2024-01-26 08:00:00+0800
categories: ["编程", "存储"]
tags: ["存储"]
weight: 4
---

今天写一个SQL时需要使用*开窗函数*，结果发现Mysql在8.0后才开始支持CTE和开窗函数.

我们目前所使用的Mysql版本比较低，所以想了一种通过子查询来模拟的方法， 记录一下。


举个例子：

现在有一张普通的表 table_age，只有三列：ID, name, age.

现在想让查询结果的每一行都带上年龄的总和，也就是实现类似开窗函数的效果

在Mysql 8.0之后 直接用开窗函数就行
```sql
SELECT
    SUM(age) OVER() AS '总年龄',
    table_age.*
FROM
    table_age;
```


在Mysql 8.0之前， 可以像下面这样写：

```sql
select 
	tempT.totalAge as '总年龄',
	table_age.*
from 
	table_age,
	(select sum(age) as "totalAge" from table_age) tempT
```

其他开窗函数也可以用这样子查询的方法，去模拟实现。

