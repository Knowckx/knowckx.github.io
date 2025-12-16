

## sitemap的几个研究


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


### 疑问解答:
Q 为什么Goole里抓取的文件不是一个纯正的xml, 包含了 XML to HTML conversion for
A hugo/github都没问题，他们提供的sitemap.xml是对的。
但是google的浏览页自己把xml自动转换成了html 所以出现了:
<!DOCTYPE html>
<!--XML to HTML conversion for https://knowckx.github.io/sitemap.xml -->
<head>

[富媒体测试器](https://search.google.com/test/rich-results)
这个工具里，只要能成功抓取 [text](https://knowckx.github.io/sitemap.xml)
点击 “查看测试页面”按钮，检查是否显示 XML 源代码 。如果显示 XML 源代码，则表明 Google 能够抓取！



### 网上的资料:

[非常有价值](https://github.com/squidfunk/mkdocs-material/discussions/7478#discussioncomment-10451113)
由于 GitHub Pages 的设置方式，所有内容都存储在有限数量的服务器和有限数量的 IP 地址下，因此这导致达到配额阈值并且 Google 完全暂停抓取服务器。

由于抓取速度慢，网站的任何问题都会被“放大”，而当网站出现太多问题时，谷歌就会放弃它。

实际的原因就是:
GitHub（IP 地址）服务器的配额上限已达到，您的抓取将被推迟。
GitHub 的免费托管服务会损害您的索引。

