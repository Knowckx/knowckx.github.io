# 常用命令
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
slug: Hugo permalinks 中文URL slug 
description: 文章的简短描述，用于 SEO 和列表页面。
slug:      "openkruise-column"  ？？ 好像是为了防止标题变成中文
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

