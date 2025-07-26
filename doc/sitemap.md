

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



## 切换到github.io后 发现sitemap没有效果了

最近有很多帖子表明，Google 连接到托管在 github.io 上的任何站点地图都存在更实质性的问题。
也就是说，问题来自github.io

--> Github 屏蔽了 Google Bot


从 Github 页面切换到 Cloudflare 页面


## 网络上的方案
[text](https://support.google.com/webmasters/thread/184533703/are-you-seeing-couldn-t-fetch-reported-for-your-sitemap?hl=en&sjid=6967765409982552168-EU)
google那边是有太多的地图积压待处理，他那边有个问题，就是待处理的地图也会被视为`无法抓取`
只有控制台给了你更具体的信息的时候，才表示这个sitemap真的有问题。

还有一种常见的误解，认为站点地图对索引非常重要。但事实并非如此（尤其对于内部结构良好的小型网站而言）。谷歌机器人是一个非常强大的蜘蛛程序，一旦它进入你的网站，并假设页面以合理的方式相互链接，它完全有能力索引整个网站，甚至不需要站点地图。

[富媒体测试器](https://search.google.com/test/rich-results)
这个工具里，只要能成功抓取 [text](https://knowckx.github.io/sitemap.xml)
点击 “查看测试页面”按钮，检查是否显示 XML 源代码 。如果显示 XML 源代码，则表明 Google 能够抓取！


### 为什么地址的内容不对？
富媒体测试器里 查看测试页面的结果是一个HTML网页？？


所以，原理是你的sitemap.xml, 最原始的内容就是一个html页面。
但是在浏览器打开时，他会发现这一行，然后自己帮你转换。
但是goole抓取的时候，他认为这是一个http页面，而看不出里面的xml结构




### githubpages 
Hugo 生成的 sitemap.xml 文件本身没问题，但在 GitHub Pages 这个服务器上，它被“处理”了。
github返回的是:

<!DOCTYPE html>
<!--XML to HTML conversion for https://knowckx.github.io/sitemap.xml -->
<head>
<title>https://knowckx.github.io/sitemap.xml</title>
</head>
<body>


### 描述你的问题
为什么Goole里抓取的文件不是一个纯正的xml, 里包含了  XML to HTML conversion for