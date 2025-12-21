+++
title = "解决AMD CPU 长时间待机自动重启的问题"
slug = "Solving-AMD-CPUs-automatically-restarting"
description = "解决AMD 7840H/8745H/8845H 长时间待机自动重启的问题"
categories = ["修电脑"]
tags = ["修电脑"]
keywords = ["修电脑", "自动重启", "AMD", "机械革命无界15x"]
weight = 5
date = "2025-12-21 10:24:38+0800"
+++



## 场景
我的笔记本是 机械革命无界15x暴风雪 CPU是AMD R7-H-255，也就是7840H/8745H/8845H的马甲U

最近遇到的问题是笔记本在长时间待机后，大概2个小时之后就会自动重启，

对应的Windows系统事件是41，也就是非正常关机

![alt text](https://pic3.zhimg.com/80/v2-f6e116f4edf599212960303f7ff33944_1440w.webp)

这很恼火，因为我有服务跑在上面需要7x24小时运行。

## 解决

然后我查了一圈，最后发现是一个复杂问题，和系统/BIOS/芯片组驱动都有关系，核心因为是CPU的负载太低时，会重启。 经过我的最后测试，发现一个最简单的解决方案:

卸载AMD的驱动软件，安装最新的25.12.1版本，然后点开管理更新-> 更新AMD的芯片组驱动（Chipset Drivers）


![alt text](https://pic3.zhimg.com/80/v2-8042588b15ff6be58af8071c01a095ba_1440w.webp)


更新完成后，经过我这一周的测试，没有异常重启。

希望对你们有用。


