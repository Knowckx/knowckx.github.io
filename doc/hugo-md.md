### weight 优先级

橙色 = 2 = 置顶(动态)
紫色 = 3 = 很有价值
蓝色 = 4 = 优质
绿色 = 5 = 价值不高 技术文章 
白色 = 6 = 日常记录
灰色 = 8 = 测试用文章

## 文章前部的元数据

title: 文章标题 (必填)
slug: hello-world  # 自定义英文的URL 标题是中文的时候 推荐设置
description: 文章的简短描述，用于 SEO 和列表页面。
keywords:  关键词列表，用于 SEO
categories: [ "技术", "Hugo" ]  # 文章所属的分类，可以使用列表：
tags: [ "SEO", "静态站点", "博客" ]  # 文章的标签，用于归类和搜索。可以使用列表：
date: 2023-10-27 08:00:00+0800  # 文章时间
weight: 5 # 排序权重。  数字越大，排名越靠前。可以用于置顶文章。
draft: true 表示文章是草稿，不会被发布

- 次要
aliases: 文章的其他 URL 别名，用于重定向旧的 URL (例如，aliases: [ "/old-post-url" ])。
expiryDate 或 publishDate: 文章过期日期或明确的发布日期（如果需要与 date 区分）。
images:  文章中使用的图片 URL 列表，用于 SEO 和社交分享。
featuredImage:  文章的特色图片 URL。
menu:  指定文章是否显示在导航菜单中，以及在哪个菜单中显示。



