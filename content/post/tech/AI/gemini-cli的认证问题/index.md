+++
title = "一行命令解决gemini-cli在国内卡认证的问题"
slug = "gemini-cli-auth-proxy-solution"
description = "在国内网络环境下，gemini-cli的OAuth网页认证经常失败..."
categories = ["AI"]
tags = ["Gemini", "AI", "Proxy"]
keywords = ["AI", "gemini-cli", "认证失败", "代理设置", "terminal proxy"]
weight = 3
date = "2025-07-15 11:12:34+0800"
+++

`gemini-cli` 是 Google Gemini 官方提供的一款强大的命令行工具，但是我自己在使用时，第一步——**认证授权**时就遇到了问题。


### 长话短说

我分别了测试了几种的挂梯子方式，最后的结论是:  
> `gemini-cli`的auth流程依赖于启动浏览器进行OAuth 2.0认证流程，整个流程不仅需要你在浏览器有科学上网环境，终端内部也需要通过`HTTP_PROXY`来实现科学上网

进一步说，为什么终端需要挂代理？  
因为`gemini`命令会启动一个本地Web服务器来接收`Google的回调`，并启动浏览器，所以整个过程都需要能顺畅访问Google服务。

### 解决方案：为你的终端设置代理

1. 请先在你的代理工具（如 Clash, V2RayN, Surge 等）中找到 HTTP 代理的端口号，通常是 `7890`, `10809` 等。
2. 然后根据你的不同操作系统输入以下命令

#### Windows (PowerShell)

```powershell
$env:all_proxy="socks5://127.0.0.1:7890"
$env:HTTP_PROXY="http://127.0.0.1:7890"
$env:HTTPS_PROXY="http://127.0.0.1:7890"
```


#### Linux/MacOS

```BASH
# 为当前终端会话设置代理
export HTTP_PROXY="http://127.0.0.1:7890"
export HTTPS_PROXY="http://127.0.0.1:7890"
# (可选，但推荐)
export all_proxy="socks5://127.0.0.1:7890"
```


