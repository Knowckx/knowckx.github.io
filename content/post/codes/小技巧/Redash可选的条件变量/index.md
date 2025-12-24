---
title: Redash可选的条件变量
description: Redash可选的条件变量
date: 2022-06-15 08:00:00+0800
categories: ["编程技巧"]
tags: []
weight: 5
---


## metabase
以前使用过Metabase作为可视化分析工具，其中有一个特性就是作为转入参数的变量是可以被定义为可选的。   
适应于有些写在where的条件但有时不需要被执行的子条件。

```sql
select * from table where name = {{valName}} [[and age = {{valAge}}]]
```

在上面这个例子里，valAge假如不填值时整个`and age = {{valAge}}`语句就不会被执行。

## redash
我们项目目前选了redash作为数据分析工具，主要是看重他的开发语言是python，方便以后的二次开发。   
但是我今天发现Redash默认不支持类似metabase字段筛选option的功能。

这很麻烦，因为有很多图表刚打开时就是需要一个默认的空白值存在。  
而在redash里要实现这个功能，我今天想了一下需要一点SQL技巧，这里记录一下。 

在DropList中加入一个自定义选项，比如月份除了1~12这12个数字外，需要加上一个`AlL`值。
每一个需要可选性的变量都按这下面的方式写

```sql
SELECT * FROM table where 1=1 and  ('{{ valMonth }}'= 'ALL' or month = '{{ valMonth }}')
```

这样，当用户的默认输入选项值配置为`AlL`时，实际上这个筛选项就没有生效。  
即:通过SQL的语法短路间接实现可选筛选项的功能

虽然丑了点，但是确实是可以工作的。

其实这个特性在BI工具里非常常见，Redash至今没有支持也是一个遗憾。