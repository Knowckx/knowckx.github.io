+++
title = "Shadcn的一个坑 forwardRef问题"
slug = "Shadcn-React.forwardRef"
description = "今天被一个shadcn-ui的问题坑了3个小时……"
categories = ["编程", "前端"]
tags = ["前端", "Shadcn", "React19"]
keywords = ["Shadcn", "React.forwardRef", "React19"]
weight = 2
date = "2025-04-27 01:04:49+0800"
+++


# 场景
现在是晚上1点，今天被一个`shadcn-ui`的问题坑了3个小时……

啊， 我人生宝贵的三小时，够我打~10把红警小块地~或者~魔兽世界打完一个完整的团队本~。 

这里记录一下，假如你们遇到了，希望能节约一点你们的时间！

## Shadcn获取组件ref


`Shadcn`增加UI组件的命令我想大家都知道

```
pnpm dlx shadcn@latest init
pnpm dlx shadcn@latest add input
```

今天我遇到一个需求，在input组件加载后实现输入框自动聚焦，因此需要拿到input组件的`ref`

然后神奇的事件来了，我发现我没办法拿到`shadcn`组件的ref  
因为使用了`pnpm dlx shadcn@latest add input`后  
他生成的input代码长这样:

```typescript
import * as React from "react"

import { cn } from "@/lib/utils"

function Input({ className, type, ...props }: React.ComponentProps<"input">) {
  return (
    <input
      type={type}
      data-slot="input"
      className={cn(
        "file:text-foreground placeholder:text-muted-foreground ...",
        className
      )}
      {...props}
    />
  )
}

export { Input }
```

发现没，里面压根没有`React.forwardRef`  
要知道`React.forwardRef`这条命令是在 React v16.3.0 版本中正式推出的，所以`shadcn`早就应该带这个引用了

# 排查

## 代码对比

然后我去官网比对了下代码版本:[官网的Input组件](https://ui.shadcn.com/docs/components/input)  
点开`Installation - Manual`，这里是他手动安装的版本:

```typescript
import * as React from "react"
 
import { cn } from "@/lib/utils"
 
const Input = React.forwardRef<HTMLInputElement, React.ComponentProps<"input">>(
  ({ className, type, ...props }, ref) => {
    return (
    // ...
```

可以发现他的版本是有`React.forwardRef`的。

这说明，我的代码版本不对。

## 排查缓存问题：

代码文件是`pnpm dlx shadcn@latest add input`这条命令生成的。  
首先想到可能是 `pnpm dlx` 拉代码时自身可能有缓存。

执行`pnpm store prune`，删除整个`node_modules`，重新安装，没解决问题。

## 排查 Registry

我很奇怪，难道是pnpm dlx 因为某种原因没有拉取到最新的 CLI 版本？  
pnpm对应的`Registry`给了我旧版本的代码？

找了下查和改Registry的方式  

```typescript
pnpm config get registry
pnpm config set registry https://registry.npmmirror.com/ --global
```

我原来用的腾讯源，换到了淘宝源

拉下来的组件代码还是没有带`React.forwardRef`！

又换了次官方源。还是一样。

问了下Gemini, 他让我直接改源码，在源码里加上`React.forwardRef`实现传递ref  
因为这样的话相当于改了自动工具生成的文件，显然这个方案不好。

## Tailwind v4 + React19


这时候脑子就抓狂了，感觉这事也太奇怪了。  耐着性子继续查(~反正老子今天不上班~)。

为了缩小可能的范围，我准备创建一个新的最小包含Shadcn的mini项目来实验:

[shadcn官网的安装流程](https://ui.shadcn.com/docs/installation/vite)

按照官网的说明，重新安装的过程中，我发现了一个细节， Shadcn官网提到了 `Tailwind v4`和`React19`的升级问题。

我的本地版本是`Tailwind v4`和`React 18.3`  
因为`React19`是前几个月刚出的，我没更新，怕不稳定。(我习惯于用**上一个大版本的最后一个小版本**)


然后我开始检查`React19`和`React.forwardRef`的关系，然后发现了一个重要信息:

[react官方的React.forwardRef](https://react.dev/reference/react/forwardRef)

这个API竟然已经被`React19`打上了Deprecated！终于有了线索。

后面接着查，我发现了这个[shadcn-ui的issue](https://github.com/shadcn-ui/ui/issues/6739)，事情大体清楚了。

# 真相大白

到了新的`React19`这个版本后，想获取组件的ref不需要再使用`React.forwardRef`  
而shadcn为了支持`Tailwind v4`和`React 19`，已经完成了代码更新。  
包括shadcn官网目前提供的`Installation`说明, 实际上是支持了`React 19`的版本

而在用户这一边，当用户增加新的UI组件时，使用的默认命令行是 
> pnpm dlx shadcn@latest add input


因为用户本身输入的要求是`shadcn@latest`。  
既然是`latest`，也就把最新的支持`React19`的版本，没有使用`React.forwardRef`的代码给用户了。

坑点就是 
- `dlx shadcn@latest`这个命令行不会识别你用的react具体版本，自动把最新的组件代码给你了
- 官网的`Installation - Manual`没同步更新也没说明，使用的反而不是react19版本, 和CLI代码不同。

## 解决方案 

解决方式有两种

1. 别用`shadcn@latest`，而是指定一个旧版本的组件。一切依然岁月静好。

2. 激进。升级`react19`。 我是本地小项目，所以直接就升级了。我把升级的命令行贴一下:


```
// 升级react19 只推荐有把握的小项目直接升级

pnpm install --save-exact react@^19.0.0 react-dom@^19.0.0

pnpm install --save-exact @types/react@^19.0.0 @types/react-dom@^19.0.0

pnpm update
```

新版本`React19`获取组件的ref很简单

```typescript
const inputRef = useRef<HTMLInputElement>(null);

// 组件的Props
const InputProps = {
    ref: inputRef // rect19 Props里可以直接传ref
};
return (
    <Input 
        {...InputProps}
    />
);
```

## 感想:

程序员的很多知识和技能，特别是对`生产工具/框架`相关的，贬值真的很快。

假设我们有一位前端程序员，学习使用React工作了一段时间，他获得了如下的开发经验:
> 在 React 中，如果你想获取一个函数组件内部渲染的 DOM 元素的 ref，这个函数组件必须使用 `React.forwardRef` 来包装。  
> 否则，直接传递 ref prop 给这个函数组件是无效的（React 会发出警告），因为函数组件本身没有实例，ref 的目的是指向底层的 DOM 节点或类组件实例。

而在2025年的4月的今天，这个经验已经**贬值为0**。

这也是很多程序员永远会**疲于奔命**的原因，主流的开发技能，这部分的知识更新速度是很快的。   
不仅自身需要时时学习，更何况现在还有**AI编程**的入场。 真的是脚步停下来就会被淘汰。

收工！





