---
title: "golang获取一个周期的时间范围"
date: 2021-10-09T17:11:10+08:00
draft: true
Categories: [golang]
---

最近因为工作的原因在对接Azure的API，其中常常遇到一些接口需要传入的参数是某一周或者某一月起点和终点的时间戳。

写多了以后我想把这个事抽出来单独写个包，用来产生当天，当周，当月的时间范围，或者输入一个时间点输出那天所在的天，周，月的时间范围。

欢迎大家直接调（懒惰是程序员进步的源泉233）

输入一个时间点，求该时间点所在的一天，一周，一月的起止时间范围， 

> go get -u http://github.com/Knowckx/asukatime 

获取某天的时间范围
``` go 
input := time.Now()  // 2021-07-20 17:37:21
tr := asukatime.GetDayRange(input)
fmt.Println(tr.Start)  // 2021-07-20 00:00:00
fmt.Println(tr.End)  // 2021-07-20 23:59:59
```

获取某周的时间范围（默认周一是第一天）
``` go
input := time.Now()  // 2021-07-20 17:37:21
tr := asukatime.GetWeekRange(input)
fmt.Println(tr)    // [2021-07-19 00:00:00, 2021-07-25 23:59:59.999]
```

获取某月的时间范围

``` go
input := time.Now()  // 2021-07-20 17:37:21
tr := asukatime.GetMonthRange(input)
fmt.Println(tr)   // [2021-07-01 00:00:00, 2021-07-31 23:59:59.999]
```
