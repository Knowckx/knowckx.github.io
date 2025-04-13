---
title: git rebase有个坑
description: git rebase有个坑
date: 2023-04-28 08:00:00+0800
categories: ["编程"]
tags: ["git"]
weight: 2
---

### 场景:

这是个老问题，以前没注意，今天手动做了几次实验，基本搞清楚了。

常见场景，自己fork了一个分支进行开发，然后发起了PR  
结果发现fork的分支已经落后于主分支几个更新。(在你的开发期间，主分支有新合入的改动)

这时候你想把主分支的改动更新进来。  
为了更**优雅**, 此时执行`git reabase upstream main`  
你把远程分支的更新内容拉取到了你本地

问题来了，你会发现你的这个分支push不上去

### 原因:  
此时你在本地分支执行git rebase之后，rebase操作修改了你的本地分支历史

> rebase节点后, Commit ID是会变的

导致本地分支与 rebase 前的远程分支历史产生分歧（即使你们的最终代码是一致的）

这时候执行push远程，git认为你的远程分支和本地分支状态不一致了, 所以rejected.


### 解决:  
推荐在使用fork的分支开发时，使用merge策略来抓取远程改动会方便一点。   
缺点: commit的历史线会比较混乱，不好看

或者在使用reabse时，搭配`-f`来强行更新**远程自己的分支**，  
这时候可以实现，你的改动完全是在别人的改动之后新加的改动，Commit ID的历史是一条直线，很会**优雅**


### 备注 rebase原理:

1. 同步状态: 开始时，本地分支 my-feature 和远程分支 origin/my-feature 指向同一个 commit，历史完全一致。

2. 执行 `git rebase upstream/main`: 这个操作做了几件事：
- 找到 my-feature 和 upstream/main 的共同祖先 commit。
- 把 my-feature 分支上 相对于共同祖先 的所有 commits "暂存" 起来。
- 把 my-feature 分支的起点移动（"变基"）到 upstream/main 的最新 commit 上。
- 把之前 "暂存" 的那些 commits 依次重新应用 到新的起点上。

3. 重点: Commit ID 的变化  
即使代码改动内容完全一样，但由于这些 commit 的父 commit 变了（它们现在是基于 upstream/main 的最新 commit，而不是之前的某个 commit），  
Git 会为它们生成 全新的 Commit ID (SHA-1 hash)。  
所以，你本地 my-feature 分支上的 commit 历史被*重写*了。都变了。

4. Push 被拒绝了:  
当你执行 git push origin my-feature 时：  
Git 对比你本地的 my-feature 和远程的 origin/my-feature。  
它发现远程分支 origin/my-feature 的历史 不是 你本地 my-feature 历史的一个 祖先 (fast-forward 关系)。

他们分叉了——远程是旧的历史，本地是 rebase 后的新历史。  
他们是两条实际上有相同的改动，但是Commit ID 不同的历史线。(rebase的坑)

