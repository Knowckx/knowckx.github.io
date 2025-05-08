+++
title = "hugo的lastmod更新问题"
slug = "hugo-lastmod-update"
description = "sitemap.xml里的lastmod什么意思"
categories = ["blog"]
tags = ["blog"]
keywords = ["hugo", "sitemap", "lastmod"]
weight = 4
date = "2023-11-29 17:12:34+0800"
+++

# 场景

最近我改了几篇旧文章之后，发现google没有对我的`更新内容`进行及时收录  
我查了一下原因，发现hugo自动生成的`sitemap.xml`里，  
对于我手动更新了内容的那篇文章，他的`lastmod`并没有变化。

## 关于lastmod字段
lastmod字段的作用，是告诉搜索引擎的`爬虫`，这个页面最后的修改时间。  
假如你的页面实际内容改了，但是这个值没有更新，就会导致爬虫判断这个页面没有改动    
这就导致了搜索引擎没有及时收录你这次的改动(其实最终会收录，但是时间会很久)

## hugo的lastmod时间

Hugo在生成`sitemap`时，确定<lastmod>值有一个顺序，优先级从高到低：

1. 文章头部`Front Matter`里指定的`lastmod`字段
``` Yaml
---
title: "我的文章"
date: 2023-01-01T10:00:00+08:00
lastmod: 2024-03-15T14:30:00+08:00 # Hugo会用这个
---
```

2. 文章头部`Front Matter`里指定的`publishDate`字段  
如果找不到 lastmod，Hugo会查找 publishDate

3. 文章头部`Front Matter`里指定的`date`字段  
如果以上两者都没有，Hugo会使用 date 参数

4. Git 提交信息 (如果 配置项里`enableGitInfo` 为 true)：   
如果你的项目是一个Git仓库，并且你在Hugo的配置文件 (hugo.toml 或 config.toml) 中设置了 enableGitInfo = true，  
Hugo会尝试使用最后一次影响该文件的Git提交的作者日期 (author date) 作为 lastmod。这通常是最准确的自动方式。  

5. 文件系统修改时间 (Fallback)：  
如果以上都不可用，它可能会尝试使用文件的实际修改时间，但这个显然在跨系统或CI/CD环境中不可靠。


## 最后解决方案

方式1 每次改文章 手动维护 lastmod 中的时间日期

方式2 在配置中指定 使用git记录的文件修改时间 or 系统的文件修改时间


``` toml
# config.toml
enableGitInfo = true  # 启用GitInfo支持

[frontmatter]
  lastmod = ['lastmod', ':git', ':fileModTime', 'date'] # 按顺序依次获取
``` 

[具体的配置解释参考](https://gohugo.io/configuration/front-matter/)

:git 从文件的 git 提交记录获取

:fileModTime' 从文件修改时间获取


