---
title: 为Terminal设置代理
description: 为Terminal设置代理
date: 2024-12-05 08:00:00+0800
categories: ["编程", "shell"]
tags: ["shell"]
weight: 7
---

Mac在终端下通过设置环境变量来配置代理，Linux的终端也是一样，这里记录一下。

打开你的代理软件，我用的是ClashX.  
找到软件的代理端口，我这边http和socks5都是7890


终端里输入:

```shell
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890
```

设置好代理后，对该终端下执行的二进制都会生效，比如golang开发中的go get

设置好之后可以用**curl**测试一下

```shell
curl google.com

# 输出HTML
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```