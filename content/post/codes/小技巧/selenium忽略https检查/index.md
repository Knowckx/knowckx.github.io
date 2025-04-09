---
title: selenium设置Chrome忽略https证书
description: selenium设置Chrome忽略https证书
date: 2024-05-05 08:00:00+0800
categories: ["编程", "爬虫"]
tags: ["爬虫"]
weight: 7
---

最近在用`selenium`写一个内部站点的自动化工具，自动填资料注册账号然后激活啥的。

可我们这个站点有时候证书会签不出来，这会导致Chrome连接时出现TLS安全检查的Warning。

虽然证书是有问题的，但是我们还想要继续访问，因此要设置忽略https的TLS检查。  
然后我在网上搜这部分的配置，网上的示例全是JAVA的，  
我这边用的golang的API，各种文档找了`1个小时`，最终跑起来了。

代码很简单，但是想把代码片段留一下，希望可以帮助需要的人节约一点小小的时间，不要重复浪费我这一小时

```go
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