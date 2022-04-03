---
title: "selenium设置Chrome忽略https证书"
date: 2021-08-01T19:13:07+08:00
Description: ""
Tags: [selenium]
Categories: [爬虫]
DisableComments: false
---


最近在用selenium写一个内部站点的自动化工具，自动填资料注册账号然后激活啥的。  
可这个站点有时候证书签不出来，就会导致Chrome连接时出现TLS安全检查的Warning。  
虽然证书是有问题的，但是我们还想要继续访问，因此要设置忽略https的TLS检查。   

我搜一下代码，很多网上的示例都是JAVA的，我这边用的golang的API，各种文档找了两个小时，最终跑起来了。

把代码片段留一下，希望可以帮助你们节约一点小小的时间~

``` go 
// 忽略TLS检查
func SetClientIgnoreTls() (selenium.WebDriver, error) {
	caps := selenium.Capabilities{
		"browserName": "chrome",
	}
	prefs := map[string]interface{}{
		"acceptInsecureCerts": true,
	}
	chromeCaps := chrome.Capabilities{
		Prefs: prefs,
		Path:  "",
		Args:  []string{"--ignore-certificate-errors"},
	}
	caps.AddChrome(chromeCaps)
	remoteAddr := "127.0.0.1:9515"
	return selenium.NewRemote(caps, remoteAddr)
}
```


