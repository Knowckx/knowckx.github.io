+++
title = "联想笔记本Fn+Ctrl键会唤醒cortana的问题"
slug = "Disabling-Fn+Ctrl-Lenovo-Laptops"
description = "介绍如何禁用联想的Fn+Ctrl快捷键"
categories = ["修电脑"]
tags = []
keywords = ["Lenovo", "Fn+Ctrl", "cortana"]
weight = 5
date = "2025-04-30 12:00:00+0800"
+++


## 场景: Ctrl+Fn唤醒小娜的问题

联想的笔记本，按住键盘上的`Ctrl+Fn`后，会尝试唤醒`微软小娜`。  
假如你的电脑已经卸载了小娜，那么就会出现一个弹窗，提示:
> 需要使用新应用以打开此ms cortana2

这是因为小娜卸载后，他找不到这个应用，引导你去windows的应用商店下载。



## 解决方案

这个问题很烦人，所以我花了1小时多搜了一圈资料，找到了解决方案。

有两个方式可以解决，可以试一下:

**方案1 直接卸载`Lenovo Hotkeys`**

`Lenovo Hotkeys`是联想的一个应用，可以从win10商店里下载。    
这个应用管理了Fn键的效果，还有开启大小写键的面板提示，开启`NumLock`键的面板提示。

卸载:  
进入Windows设置（快捷键: Win+I）> 应用 > 搜索:`联想快捷键` 或者 `Lenovo Hotkeys`  > 卸载

**方案2 关闭对应联想的服务**

- 快捷键`Win+S`, 输入`服务`, 打开Win的`服务`窗口
- 在服务里找`Lenovo Function and Fn Key Service`。
- 右键属性 -> 点击`停用` 然后启动类型改成`禁止`

此时应该 `Ctrl+Fn` 不会触发了。测试后重启电脑再次确定一下。

## 禁用后的好处:

解放了Home键和End键。 在编辑长文件时，可以试下这两个快捷键:

快捷键<kbd>Ctrl</kbd> + <kbd>Fn</kbd> + <kbd>左箭头</kbd>   -> 实现跳转到文件顶部（Home键功能）

快捷键<kbd>Ctrl</kbd> + <kbd>Fn</kbd> + <kbd>右箭头</kbd>   -> 实现跳转到文件末尾（End键功能）


---

## 备注:联想的Fn键的其他效果

Fn+空格  控制键盘背光

Fn+M  启用/禁用触摸板

Fn+Q  不同操作模式间切换（节能、性能、智能）

Fn+Esc 开启和关闭Fn














