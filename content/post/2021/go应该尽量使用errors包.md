---
title: "go不要抛裸错误 使用errors包"
date: 2021-04-11T17:27:14+08:00
Description: ""
Tags: [go]
Categories: [go语言]
DisableComments: false
---

Go目前对错误处理的支持太简单了，这导致排查错误不方便。应该使用errors包带上栈信息。

<!--more-->

---

go语言最常见错误处理是一层一层向上抛错误，但是直接`return err`这样是不好的，  
因为最上层打印错误的时候只有一行错误字符串，中间的错误栈全部丢失了，这不方便定位错误在代码中的传递过程。  
我发现身边不少写了3年以上的go开发并未注意到这一点, 感觉很多人都没养成好习惯。  

所以推荐日常工作中应该尽量使用errors包带上栈信息。
包：`"github.com/pkg/errors"`  
这个包最大的作用就是用error加上stack记录

注意：
- 打印错误的时候需要%+v 才能打印出stack信息
- 假如你的日志包用的`zerolog`，需要配置成这样  
`zerolog.ErrorStackMarshaler = pkgerrors.MarshalStack`
- 假如想判断底层错误就使用`Cause`方法


示例代码：

``` go
package main

import (
	"fmt"
	"testing"

	"github.com/pkg/errors"
)

func Test_Errors(t *testing.T) {
	err := GetErrorLayer2()
	fmt.Printf("%+v\n", err) // 必需是%+v 才能打印出stack信息

}

func GetErrorLayer2() error {
	err := GetBaseError()
	return errors.WithStack(err)
}

func GetErrorLayer1() error {
	err := GetBaseError()
	return errors.WithStack(err)
}

func GetBaseError() error {
	return fmt.Errorf("base error")
}
```


输出结果：
```
=== RUN   Test_Errors
base error
github.com/Knowckx/Asuka/QuickTest.GetErrorLayer2
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

第11行，第18行正是通过WithStack包装错误的地方。