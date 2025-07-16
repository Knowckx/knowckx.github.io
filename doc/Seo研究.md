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

```

意思:
User-agent：指定要针对的爬虫，如“*”表示对所有爬虫生效。
Disallow：指定禁止访问的路径或文件。
Allow：指定允许访问的路径或文件。


### 几个细节

- 文件夹的名字可以随便你取 最后的文章title还是使用元数据的title
- 中文标题， 中间有一个空格，他会自动为URL加一个'-'连接，不用操心
- 全中文标题，会被自动转成%URL 不需要操心

解释: 为什么全中文标题的文章URL很奇怪
URL 只能包含有限的 ASCII 字符集
当 URL 中需要包含非 ASCII 字符（如中文、日文、特殊符号等）或在 URL 中具有特殊含义的字符（如空格、&、? 等）时，必须对其进行编码
最常见的方法是百分号编码 (Percent-Encoding)，也就是你看到的 %E4%B8%AD%E6%96%87%E6%B5%8B%E8%AF%95444。
这是将 "中文测试444" 这几个字的 UTF-8 字节表示转换为 % 加上两位十六进制数的形式
现代浏览器在地址栏中通常会 显示 解码后的 URL（即直接显示中文）

- 怎么获取中文转码后的地址？
浏览器地址栏中复制地址后 粘贴到google网址检查里

### 影响 Google 收录/更新速度的原因

理想情况: 对于老牌、权威、权重较高、更新频繁的网站，更新的内容可能在几小时到一两天内被 Google 重新抓取和索引。
一般情况: 对于普通网站，可能需要几天到一周的时间。
较慢情况: 对于新站、低权重或更新不频繁的网站，可能需要一周以上甚至几周的时间。


- 调 google API 让它收录
这样比爬虫收录快很多



## sitemap的几个研究

### 操作台



### 用好`网址检查`功能
网址检查不仅可以知道收录了没有，还能知道是不是来自sitemap的收录

## 我的改动
1. 不能启用多语言博客，因为会导致中文的那个二级的sitemap.xml变成了纯文本而不是xml格式！
2. 设置sitemap里的lastmod
3. 配置里加上sitemap配置！
[sitemap]
  filename = 'sitemap.xml'
  changefreq = 'weekly'
  priority = 0.5

## 追踪

### 目前问题:
- 主动重复提交sitemap 可以立马实现刷新
在GSC里重复提交站点地图，他会立马刷新读取时间
此时你的新博文变成了: `已发现 - 尚未编入索引`
但是过2~3天后没有进入索引  变成了 "未检测到任何引荐站点地图"

### 核心: sitemap还是没有起效
[text](https://knowckx.github.io/disabling-fn-ctrl-lenovo-laptops/)  显示未检测到任何引荐站点地图

[](https://knowckx.github.io/seo-test-148/) 没有收录

### 测试
[把直接加入到网址检查里](knowckx.github.io/sitemap.xml)
移除robots.txt 站点地图可以被robots.txt文件的规则屏蔽

如果站点地图位于 http://www.example.com/mysite/sitemap.xml，则该站点地图的以下网址将无效：
http://www.example.com/ - 级别高于站点地图
http://www.example.com/yoursite/ - 与站点地图处于同级目录中（必须前往上层目录，然后再向下返回此网址所在的目录，才能获取此网址）。


日期必须使用 W3C 日期时间编码。日期时间格式：
2005-02-21  或者   
2005-02-21T18:00:15+00:00