---
title: "关于svn还原和重置操作"
date: 2020-08-27T17:02:56+08:00
Description: ""
Tags: [svn]
Categories: []
DisableComments: false
---

git重置本地修改很方便，可以`git reset --hard`   
那svn的操作呢？

<!--more-->

---

在网易游戏工作时，部门里为了方便与策划同事的协作，项目的代码和配置表格都使用了SVN作为版本控制工具。  
用惯了Git的我其实不太适应，比如工作中改了几个文件，现在想重置回来，假如git就很好做了。
> git reset --hard

Svn就只能在界面上右键，然后手动一个一个还原，太麻烦了。  
作为懒人是不会满足于这种效率的。然后我就去查了一下，发现Svn也是有自己的命令行可以用的。  
这里记两个和还原相关的:  

```

svn info    仓库信息
svn st      查版本状态
svn revert . -R     还原所有更改
svn cleanup . --remove-unversioned  清理未被版本控制的文件
```
---