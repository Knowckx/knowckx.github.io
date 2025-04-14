
# hugo的使用

使用的模板 [text](https://github.com/CaiJimmy/hugo-theme-stack)

Q 怎么配置这个主题
<https://stack.jimmycai.com/config/menu>

## 手动更新主题

hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3
hugo mod tidy

## 本地启动

hugo server

### 优先级

橙色 = 2 = 置顶
紫色 = 3 = 很有价值的文章
蓝色 = 4 = 比较优质的文章
绿色 = 5 = 一般技术相关文章
白色 = 6 = 日常记录
灰色 = 7 = 低价值文章

# 文章前部的元数据

title: 文章标题 (必填)
description: 文章的简短描述，用于 SEO 和列表页面。
date: 2023-10-27 08:00:00+0800  # 文章时间
categories: [ "技术", "Hugo" ]  # 文章所属的分类，可以使用列表：
tags: [ "SEO", "静态站点", "博客" ]  # 文章的标签，用于归类和搜索。可以使用列表：
weight: 1 # 排序权重。  数字越大，排名越靠前。可以用于置顶文章。

- 次要
draft: true 表示文章是草稿，不会被发布
slug: "my-awesome-post"  # 文章的 URL 别名 (例如，)，如果没有设置，通常 Hugo 会根据标题自动生成。 推荐设置，特别是标题是中文的时候。
aliases: 文章的其他 URL 别名，用于重定向旧的 URL (例如，aliases: [ "/old-post-url" ])。
expiryDate 或 publishDate: 文章过期日期或明确的发布日期（如果需要与 date 区分）。

- 高级元数据和自定义参数：

images:  文章中使用的图片 URL 列表，用于 SEO 和社交分享。
featuredImage:  文章的特色图片 URL。
keywords:  关键词列表，用于 SEO (但现在 SEO 的重要性降低)。
menu:  指定文章是否显示在导航菜单中，以及在哪个菜单中显示。


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
