---
title: golang错误处理 使用errors包
description: golang错误处理 使用errors包
date: 2024-10-29 08:00:00+0800
categories: ["编程相关", "golang"]
tags: ["golang"]
weight: 4
---

我发现身边不少写了3年以上的go开发并未注意到这一点, 感觉很多人都没养成好习惯。

Go的默认写法，对错误处理的支持太简单了，这会导致排查错误不方便。

最常见错误处理方式是在调用链一层一层向上抛错误，但是每层调用处直接`return err`这样是不好的，    
因为最上层打印错误的时候只有一行错误字符串，中间的**错误栈**全部丢失了，这不方便定位错误在代码中的传递过程。


推荐日常工作中应该尽量使用**errors**包带上栈信息。 包：`"github.com/pkg/errors"`  
这个包最大的作用就是用error加上stack记录


示例代码：

```go
package main

import (
    "fmt"
    "testing"

    "github.com/pkg/errors"
)

func Test_Errors(t *testing.T) {
    err := CallFunc1()  // 第1层调用
    fmt.Printf("%+v\n", err) // 必需是%+v 才能打印出stack信息

}

func CallFunc1() error {
    err := CallFunc2() // 第2层调用
    return errors.WithStack(err)
}

func CallFunc2() error {
    err := GetBaseError() // 第3层调用
    return errors.WithStack(err)
}

func GetBaseError() error {
    return fmt.Errorf("base error")
}
```

输出结果(调用栈)：

```shell
=== RUN   Test_Errors
base error  
github.com/Knowckx/Asuka/QuickTest.CallFunc1
    /Users/i51111/dev/AsukaProj/Asuka/QuickTest/main_test.go:18
github.com/Knowckx/Asuka/QuickTest.Test_Errors
    /Users/i51111/dev/AsukaProj/Asuka/QuickTest/main_test.go:11
testing.tRunner
    /usr/local/go/src/testing/testing.go:1194
runtime.goexit
    /usr/local/go/src/runtime/asm_amd64.s:1371
--- PASS: Test_Errors (0.00s)
PASS
```


注意：

打印错误的时候需要%+v 才能打印出stack信息

假如你的日志包用的`zerolog`，需要配置成这样:  
```go
zerolog.ErrorStackMarshaler = pkgerrors.MarshalStack
```

假如想判断具体的错误就使用**Cause**方法