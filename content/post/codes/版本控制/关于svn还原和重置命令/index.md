---
title: 关于svn还原和重置命令
description: 整理 SVN 里对应 git reset --hard 的还原方式，以及清理未跟踪文件时可用的命令。
date: 2022-08-18 08:00:00+0800
lastmod: 2025-12-24T11:01:31+08:00
categories: ["编程相关"]
tags: ["svn"]
weight: 5
---


git重置本地修改很方便，可以**git reset --hard**

查了下svn对应的操作:

```shell
svn revert . -R     # 还原所有更改
svn cleanup . --remove-unversioned  # 清理未被版本控制的文件
```
