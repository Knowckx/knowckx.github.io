---
title: 关于svn还原和重置命令
description: 关于svn还原和重置命令
date: 2022-08-18 08:00:00+0800
categories: ["编程", "版本控制"]
tags: ["版本控制"]
weight: 7
---


git重置本地修改很方便，可以**git reset --hard**

查了下svn对应的操作:

```shell
svn revert . -R     # 还原所有更改
svn cleanup . --remove-unversioned  # 清理未被版本控制的文件
```
