---
title: 使用二进制来存储多个布尔值
description: 使用一个Int值来存储多个布尔值的技巧
date: 2024-04-27 10:00:00+0800
categories: ["编程", "存储"]
tags: ["数据库"]

---

最近遇到一个需求，需要在数据库保存一组配置，每个配置都是一个bool值，表示表的那一列是否开启了

按照最常见的做法，需要每有一个bool值就加一个表字段， 这样做有两个问题:
1. bool值比较多的时候，需要在数据库里对应创建的字段就比较多，打开表一看全是bool值，信息密度低。  
2. 不方便动态调整，比如上线后业务说想要加一个bool配置，数据库表加字段并不方便，特别是生产环境

这时候我想起`linux`那个用来改文件权限的命令， `chmod 777`.  
这里的777是有含意的.
简单来说，
> 1 = 001   执行权限  
2 = 010   写权限  
4 = 100   读权限  
7 = 111   全部权限  

3个7，`777`表示三种用户，都有全部权限。  
`chmod 777`意思就是把这个文件的所有权限都开放出来

按照这个思路，我们日常遇到多个bool配置的时候，也可以使用这个方式通过一个int字段来存储大量的bool值，  
唯一需要注意的是需要在代码里固定好bool字段对应的顺序。

Talk is cheap, show the code below:

```Golang
// golang code
// a lot of bool fields
type BoolConfig struct {
    IsAddName    bool //1
    IsAddAddress bool //2
    IsAddEamil   bool //3
    IsAddAge     bool //4
    IsAddPwd     bool //5
}

// ToBin return binary like: FTFFF -> "01000"
func (c *BoolConfig) ToBin() string {
    v := reflect.ValueOf(*c)
    rst := ""
    switch v.Kind() {
    case reflect.Struct:
        for i := 0; i < v.NumField(); i++ {
            val := v.Field(i).Interface().(bool)
            if val == false {
                rst = rst + "0"
                continue
            }
            rst = rst + "1"
        }
    }
    return rst
}

// binary to Decimal  1010 -> 10 | 111 -> 7
func BinDec(b string) (int, error) {
    ss := strings.Split(b, "")
    l := len(ss)
    d := float64(0)
    for i := 0; i < l; i++ {
        f, err := strconv.ParseFloat(ss[i], 10)
        if err != nil {
            return -1, err
        }
        d += f * math.Pow(2, float64(l-i-1))
    }
    return int(d), nil
}

func Test_ToBin(t *testing.T) {
    cf := new(BoolConfig)
    cf.IsAddAddress = true

    fmt.Printf("Config: %+v\n", cf)

    resBin := cf.ToBin()
    fmt.Printf("Bin: %+v\n", resBin) // 01000

    dec, err := BinDec(resBin)
    if err != nil {
        panic(err)
    }
    fmt.Printf("dec: %+v\n", dec) // 8
    return
}

```