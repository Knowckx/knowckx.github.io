---
title: yfinance使用的示例
description: yfinance使用代理访问数据
date: 2025-03-07 08:00:00+0800
categories: ["编程", "量化"]
tags: ["量化"]
weight: 6
---

### 使用yfinance获取数据

最近有需求，需要取一下美股的K线数据，搜了一圈最后选择了`yfinance`这个包，结果第一次使用就报错了

> Too Many Requests. Rate limited. Try after a while.

查了一圈最后发现，目前在国内调这个包需要挂代理了  
我把示例代码贴这里，有需要的人可以直接粘贴过去调试。

记得改成你本地的科学上网代理端口

```python
import os
import yfinance as yf

# 定义纳斯达克100指数期货的代码
ticker_symbol = 'NQ=F'


# 设置代理，指定HTTP 请求的代理服务器 | 使用你自己的代理端口
proxy = 'http://127.0.0.1:7890'
os.environ['HTTP_PROXY'] = proxy 
os.environ['HTTPS_PROXY'] = proxy

# 创建 Ticker 对象
ticker = yf.Ticker(ticker_symbol)

# 获取最新的市场数据
ticker_info = ticker.info

# 获取当前价格和前收盘价
current_price = ticker_info['regularMarketPrice']
previous_close = ticker_info['regularMarketPreviousClose']

# 计算涨幅
price_change = current_price - previous_close
percent_change = (price_change / previous_close) * 100

# 输出结果
print(f"当前价格: {current_price}")
print(f"前收盘价: {previous_close}")
print(f"涨跌额: {price_change}")
print(f"涨跌幅: {percent_change:.2f}%")


```

除了`yfinance`之外，国内的`新浪财经`也是很好的数据来源。 后面我再贴一些。