---
title: Hugo的MarkDown语法示例
description: MarkDown语法收集
date: 2025-01-01
image: cover.jpg
weight: 5
categories: ["blog"]
tags: ["blog", "MarkDown"]
---

# 各级标题

```
# H1
## H2
### H3
```

## 文字效果

### 斜体 + 加粗
这是*文字斜体*效果

```
这是*文字斜体*效果
```

这是**文字加粗**效果

```
这是**文字加粗**效果
```

### 删除线 + 下划线

这是一段~加了删除线的文本~

```
这是一段~加了删除线的文本~
```

这是一段<u> 下划线示例文本 </u>内容
```
这是一段<u> 下划线示例文本 </u>内容
```

### 超链接

[百度一下，你就知道](http://www.baidu.com)

```
[百度一下，你就知道](http://www.baidu.com)
```


## 拓展标记

### 键盘按键

<kbd>Ctrl</kbd> + <kbd>X</kbd>
```
<kbd>Ctrl</kbd> + <kbd>X</kbd>
```

### 高亮


Most <mark>salamanders</mark> are nocturnal



```
Most <mark>salamanders</mark> are nocturnal
```

- 别的编辑器可以用"`==高亮文本==`"来实现高亮，hugo好像不行

###  上下标

下标 H<sub>2</sub>O

上标 X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

```
下标 H<sub>2</sub>O

上标 X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>
```





## 引用

### 块内引用
这是`块内引用`效果

```
这是`块内引用`效果
```



### 区块引用
> 这是区块引用的效果

```
> 这是区块引用的效果
```

### 代码块

缩进推荐四个空格

```html
<p>A paragraph</p>

```



## 列表
### 有序列表

1. First item
2. Second item
3. Third item

### 无序列表

* List item
* Another item
* And another item

### 多级列表

* Fruit
  * Apple
  * Orange
  * Banana

```
* Fruit
  * Apple
  * Orange
  * Banana
```







## 图片

- 图片需要放在和`.md`文件同一个文件夹里 
- hugo一行可以引用多个图片，支持横排图片

![Image 1](1.jpg) ![Image 2](2.jpg)

```markdown
![Image 1](1.jpg) ![Image 2](2.jpg)
```



## 水平分割线

下面是两种水平分割线：

---
***

```
代码:
---
***
```



# 高级功能

## 表格

   Name | Age
--------|------
    Bob | 27
  Alice | 23


```
   Name | Age
--------|------
    Bob | 27
  Alice | 23
```

## 注释

注释的内容对用户不可见
<!-- 这里是注释的内容 -->

```html
<!-- 这里是注释的内容 -->

```

## 视频地址的引用

For more details, check out the [documentation](https://stack.jimmycai.com/writing/shortcodes).

### YouTube video

{{< youtube "0qwALOOvUik" >}}

格式:  
`{{` + `视频标签`  + `}}`

4种视频标签格式:
```
< youtube "0qwALOOvUik" >
< bilibili "BV1d4411N7zD" >
< tencent "g0014r3khdw" >
< video "https://www.w3schools.com/tags/movie.mp4" > # 通用视频地址
```




