---
title: "Redash可选的条件变量"
date: 2021-09-30T11:36:47+08:00
Description: ""
Tags: [sql]
Categories: [redash]
DisableComments: false
---

### metabase
以前使用过Metabase作为可视化分析工具，其中有一个特性就是作为转入参数的变量是可以被定义为可选的。
适应于有些写在where的条件但有时不需要被执行的子条件。
参见[metabase的可选变量](https://www.metabase.com/learn/sql-questions/field-filters.html#creating-a-filter-widget-with-a-dropdown-menu)


``` sql
select * from table where name = {{valName}} [[and age = {{valAge}}]]
```
在上面这个例子里，valAge假如不填值时整个`and age = {{valAge}}`就不会被执行。

### redash
我们项目目前选了redash作为数据分析工具，主要是看重他的开发语言是python，方便以后的二次开发。
但是其中有个问题，我今天竟然发现Redash默认不支持类似metabase字段筛选option的功能。  
这可太麻烦了，因为有很多图表刚打开时就是需要一个空白值存在。

而在redash里要达到这个功能，我今天想了一下需要一点SQL技巧，这里记录一下。
两步:
- 在DropList中加入一个自定义选项，比如月份除了1~12这12个数字外，需要加上一个“All”值。
- 每一个需要`可选性`的变量都按这下面的方式写

``` sql
SELECT * FROM table where 1=1 and  ('{{ valMonth }}'= 'ALL' or month = '{{ valMonth }}')
``` 

这样，当用户的默认输入选项值为“All"时，实际上这个筛选项就没有生效。

虽然丑了点，但是确实是可以工作的。  
其实这个特性在BI工具里非常常见，Redash至今没有支持也是一个遗憾。