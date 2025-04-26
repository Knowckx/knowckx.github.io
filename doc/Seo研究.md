## google收录

- 攻略 [text](https://blog.sugarin.net/p/google%E6%94%B6%E5%BD%95/)

- 配置 GA Tracking ID
> googleAnalytics = "G-4DX39HKR9W"

### robots.txt

Robots协议（也称为robots.txt）是一个网站根目录下的文本文件，用于指示搜索引擎爬虫哪些页面可以被抓取或不应该被抓取。

`stack`主题是不自带robots的，需要新建`robots.txt`放置`static`目录下, 

hugo是静态主题，所以没什么需要屏蔽的内容，协议写法可以参考如下

```typescript
User-agent: *
Allow: /
Disallow: /assets/

Sitemap: https://knowckx.zone.id/sitemap.xml
```

意思:
User-agent：指定要针对的爬虫，如“*”表示对所有爬虫生效。
Disallow：指定禁止访问的路径或文件。
Allow：指定允许访问的路径或文件。



### 几个细节

- 文件夹的名字可以随便你取 最后的文章title还是使用元数据的title
- 中文标题， 中间有一个空格，他会自动为URL加一个'-'连接，不用操心

- 全中文标题，会被自动转成%URL 不需要操心
解释:
URL 只能包含有限的 ASCII 字符集
当 URL 中需要包含非 ASCII 字符（如中文、日文、特殊符号等）或在 URL 中具有特殊含义的字符（如空格、&、? 等）时，必须对其进行编码
最常见的方法是百分号编码 (Percent-Encoding)，也就是你看到的 %E4%B8%AD%E6%96%87%E6%B5%8B%E8%AF%95444。
这是将 "中文测试444" 这几个字的 UTF-8 字节表示转换为 % 加上两位十六进制数的形式
现代浏览器在地址栏中通常会 显示 解码后的 URL（即直接显示中文）

- 怎么获取中文转码后的地址？
浏览器地址栏中复制地址后 粘贴到google网址检查里

- Sitemap的lastmod时间  
这个时间使用的是最后一篇文章的更新时间。  
但是我估计这个时间更新了也不会影响google对你的收录时间。

搜索结果显示这个事情好像不是很重要。没人提Sitemap+lastmod的问题



### 影响 Google 收录/更新速度的原因

理想情况: 对于老牌、权威、权重较高、更新频繁的网站，更新的内容可能在几小时到一两天内被 Google 重新抓取和索引。
一般情况: 对于普通网站，可能需要几天到一周的时间。
较慢情况: 对于新站、低权重或更新不频繁的网站，可能需要一周以上甚至几周的时间。

- URL不收录的问题:
**应该不是中文编码问题 也不是slug问题**
google报告
已经收录了 [text](https://knowckx.zone.id/p/使用二进制来存储多个布尔值/)
对应的编码URL也收录了  [text](https://knowckx.zone.id/p/%E4%BD%BF%E7%94%A8%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%9D%A5%E5%AD%98%E5%82%A8%E5%A4%9A%E4%B8%AA%E5%B8%83%E5%B0%94%E5%80%BC/)

这篇文章是没有设置对应的Slug的

- **网址检查**
`使用二进制来存储多个布尔值` 这文章在网址检查里是OK的

- **抓取统计信息**
在 Google Search Console (GSC) 的“设置” > “抓取统计信息”中查看大致的抓取频率。

## 有没有收录

**操作台**
[text](https://search.google.com/search-console?resource_id=https%3A%2F%2Fknowckx.zone.id%2F&hl=zh-CN)


- **搜索时验证**
google搜索时:  
二进制来存储 site:https://knowckx.zone.id/


## TODO

https://knowckx.zone.id/zh-cn/sitemap.xml
他04.22读取了我新的站点地图，但是今天04.24 他还没有更新里面的URL新文章

过1周/1月 再看看这个65篇的sitemap生效没

- 测试新4篇

knckx未来147 site:https://knowckx.zone.id/ 这个到可以

测试的4篇目前在google搜不到
ForSeoTest111 site:https://knowckx.zone.id/

![alt text](https://knowckx.zone.id/favicon.ico)

先保髓-再根管  site:https://knowckx.zone.id/
需要能直接定位到这篇文章