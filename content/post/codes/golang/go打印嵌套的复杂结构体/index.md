---
title: go打印嵌套的复杂结构体
description: go打印嵌套的复杂结构体
date: 2022-07-02 08:00:00+0800
categories: ["编程技巧", "golang"]
tags: ["golang"]
weight: 4
---

这个需求其实在日常写代码中比较常见，比如调一个接口返回来一个结果，  
这时想看一下这个变量有什么内容  
假如使用fmt的`%v` 或者 `%+v`, 那只能打印简单类型的结构体，对于嵌套的复杂结构体就不行了。

而使用反射去一层一层解析又太麻烦，我想找一种简单方便的方式。  
昨天中午正在吃饭时，突然想到可以通过json的Marshal方式输出变量的内容。

写一下示例代码：


``` go
type User struct {
	Name string
	Age  int
	Home *Home
}

type Home struct {
	Address     string
	PeopleLives []string
	PeopleInfo  map[string]string
}

// 输出一个结构体嵌套的变量
func GenTestUser() *User {
	ho := Home{}
	ho.Address = "beijing"
	ho.PeopleLives = []string{
		"Raman Kalita", "Abhishek Garg",
	}
	ho.PeopleInfo = map[string]string{
		"Raman Kalita":  "civil servant",
		"Abhishek Garg": "teacher",
	}
	us := User{}
	us.Name = "Alise"
	us.Age = 31
	us.Home = &ho
	return &us
}

func Test_PrintJson(t *testing.T) {
	us := GenTestUser()
	PrintJson(us)
}

// PrintJson 通过json序列化打印数据内容
func PrintJson(in interface{}) {
	res, err := json.MarshalIndent(in, "", "\t") // beautiful json
	if err != nil {
		log.Error().Stack().Err(err).Send()
		return
	}
	Printf(string(res))
}
```

输出的结果:

``` go
{
	"Name": "Alise",
	"Age": 31,
	"Home": {
		"Address": "beijing",
		"PeopleLives": [
			"Raman Kalita",
			"Abhishek Garg"
		],
		"PeopleInfo": {
			"Abhishek Garg": "teacher",
			"Raman Kalita": "civil servant"
		}
	}
}

```

可以看到Home这个结构体也打印出来了，无论是map还是slice输出都是OK的，
PrintJson这个函数才几行？ 简单好用~

