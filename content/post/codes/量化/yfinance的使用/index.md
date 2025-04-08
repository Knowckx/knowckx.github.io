---
title: yfinance的示例
description: yfinance挂代理
date: 2025-04-07 08:00:00+0800
categories: ["编程", "量化"]
tags: ["量化"]
weight: 6
---

### 使用yfinance获取数据

最近想拿一下美股的日K线，找到了`yfinance`这个包，结果第一次用这个包就报错了

> Too Many Requests. Rate limited. Try after a while.

查了一圈发现在国内调这个包需要挂代理，把示例代码贴这里

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