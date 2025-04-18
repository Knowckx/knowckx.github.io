
# 评论功能

## Giscus +3 选用

按照官方教程(https://giscus.app/zh-CN)安装，然后将配置导入到config.yaml里。
该评论系统全程托管在github discussions里，非常省心省力。

是由 GitHub Discussions 驱动的评论系统，
特性有：
开源。
无跟踪，无广告，永久免费。
无需数据库。全部数据均储存在 GitHub Discussions 中。
支持自定义主题！
支持多种语言。
高度可配置。
自动从 GitHub 拉取新评论与编辑。
可自建服务！

[text](https://www.lixueduan.com/posts/blog/02-add-giscus-comment/)

### utterances +3  基于 GitHub issues 的评论工具
类型: 基于 GitHub Issues 的开源方案
优点: 完全免费、无广告、尊重隐私，利用 GitHub Issues 存储评论，数据在自己的仓库里，轻量，设置相对简单（需要创建一个公开的 GitHub repo 并安装 App），开发者群体非常熟悉 GitHub 登录。
缺点: 评论者必须拥有 GitHub 账号并授权，功能相对基础（依赖 GitHub Issues 的功能，如 Markdown、反应），不支持匿名评论，评论样式自定义有限。
适合: 技术类博客，读者群体主要是开发者或熟悉 GitHub 的用户，高度重视隐私和免费。


### disqus 排除
这个工具配置也比较简单，也是免费的。
但是，广告多，而且加载也比较慢。
hugo本身已经内置了Disqus
优点: 功能非常全面（嵌套评论、点赞、社交登录、强大的管理后台、垃圾评论过滤），设置相对简单，用户群体庞大，很多人已有账号。
缺点: 隐私问题严重！ 免费版会展示广告并追踪用户，可能拖慢网站加载速度，界面有时与博客风格不搭，数据不在自己手中。


Remark42 (如果愿意自托管):
原因: 如果你需要比 Giscus 更丰富的功能（如匿名评论、社交登录、图片上传），同时又想保持隐私和数据控制，并且不介意投入一些精力进行自托管，Remark42 是目前功能最强大、最完善的开源自托管方案之一。
优点: 功能极其丰富，隐私保护到位，性能出色，完全免费（除服务器成本），数据完全由你掌控。
缺点: 需要技术能力来部署和维护。
结论: 对于有技术能力、追求功能与隐私完美结合的用户，这是接近“最佳”的选择。



# google收录

- 攻略 [text](https://blog.sugarin.net/p/google%E6%94%B6%E5%BD%95/)

- 配置  GA Tracking ID

googleAnalytics = "G-4DX39HKR9W"


- 操作台

[text](https://search.google.com/search-console?resource_id=https%3A%2F%2Fknowckx.zone.id%2F&hl=zh-CN)




### 增加 robots

Robots协议（也称为robots.txt）是一个网站根目录下的文本文件，用于指示搜索引擎爬虫哪些页面可以被抓取或不应该被抓取。
robots.txt文件的基本语法相对简单，它通常包含一些基本的指令，例如：

User-agent：指定要针对的爬虫，如“*”表示对所有爬虫生效。
Disallow：指定禁止访问的路径或文件。
Allow：指定允许访问的路径或文件。
Crawl-delay：指定爬取延迟时间（并非所有搜索引擎都支持）

`stack`主题是不自带robots的，需要新建`robots.txt`放置`static`目录下, 
hugo是静态主题，所以没什么需要屏蔽的内容，协议写法可以参考如下

```typescript
User-agent: *
Allow: /
Disallow: /assets/

Sitemap: https://knowckx.zone.id/sitemap.xml
```

### Sitemap的问题

Sitemap下的lastmod时间 使用的是最后一篇文章的时间。
不确定他能不能知道你更新了之前的文章。

<lastmod>2025-04-10T08:00:00+08:00</lastmod>

搜索结果显示这个事情好像不是很重要。没人提Sitemap+lastmod的问题


### 目前存在收录不全的问题

Q 怎么获取中文转码后的地址？
复制地址后 粘贴到google网址检查里

Q 标题有一个空格需要去除吗？
不需要 他自动会加一个'-'

Q 怎么判断你已经被收录了？
- 搜索时使用 site:https://knowckx.zone.id/   site:https://knowckx.github.io/
- 单篇文章搜: 分享一个自用的k8s端口转发脚本  site:https://knowckx.zone.id/


文章
https://knowckx.zone.id/p/先保髓-再根管/
Sitemap里确定是有的。
%E5%85%88%E4%BF%9D%E9%AB%93-%E5%86%8D%E6%A0%B9%E7%AE%A1
但是google收录里没有。

可能是sitemap的问题。