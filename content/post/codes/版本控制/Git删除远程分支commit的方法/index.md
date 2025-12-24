---
title: Git删除远程分支某次commit的方法
description: Git删除远程分支某次commit的方法
date: 2024-09-18 08:00:00+0800
categories: ["编程相关"]
tags: ["git"]
weight: 3
---


## 懒人阅读版

找到你需要去掉的commit的前一次的commitID

执行 
```shell
git rebase -i commit_id -X their
```


进入编辑后 在vi里 drop掉不需要的commit

```shell
git push -f  # 覆盖远程分支
```

这样远程分支的那次commit就彻底消失了。

`git push -f`是高危操作 不是特殊时刻不要用

-----

## 原文
最近遇到一个问题，我们的项目代码里，之前把一些*密码*配置明文传到了github，  
但是公司的安全策略不希望我们在代码里直接留有敏感的账号凭证信息，否则会被扫出来合规性问题。  

按照我同事之前的经历：安全部门扫出来之后就会发邮件给你老板，让你半夜起来马上消掉。  
因此产生了一个需求，怎么把github上已提交的敏感信息抹掉呢？ 

我调研了下，目前已知有几种方式：

### 通过git revert commit_id来实现
git revert 可以撤销某次操作，他是通过提交一次新的commit来回滚之前的一次commit的内容，
大部分情况下这个功能是够用的。

不适用我们的场景，因为旧的commit实际上不会消失，在github直接打开旧的commit，还是可以看到对应密码信息，不合规。

### 通过git reset --hard commit_id
直接重置到密码提交前的那一次commit，然后`git push -f`重置分支

这样做的前提条件是需要提交密码的那一次commit是最近发生的，  
不然就需要手动把后面的历史commit都补回来，工作量很大

### 通过git rebase -i 来实现丢弃一个commit
这是今天发现的解决方案，有点类似于以前git合并多个commit的操作。  
下面说下流程：

1. 找到你需要丢弃的commit_id
在我的例子里，我在`bdfb32a`这次提交里提交了密码  
这次commit的前一次是`0119c21`

所以执行 
```shell
git rebase -i 0119c21 -X their
```

解释:  
-i 进入交互模式  
-X their 方便后续的commit自动合并，不然你需要手动操作冲突。  

2. 这时进入了vi的编辑界面，此时git会把commit_id之后发生的commit列出来，

我们按i进入编辑，手动把不需要的那个commit `bdfb32a`前面的`pick`改成`drop`.

3. 然后vi和rebase的基本操作：
```
esc, :wd
git add .
git rebase --continue
```

4. 手动push 覆盖远程分支

此时你通过查看历史，比如`git log -10`  
应该可以看到commit的历史已经改变了，`bdfb32a`已经彻底消失了(被丢弃)。

执行`git push -f`  

再去github看一下 

`0119c21`之后的这次`bdfb32a`已经抹除。

收工~！