---
title: "总结一下k8s查询secret几种方式"
date: 2022-01-25T18:36:44+08:00
Description: ""
Tags: [k8s]
Categories: [k8s]
DisableComments: false
---

来Infra Team工作有一年了，总结一下目前我已知的获取k8s secret的方式


## 手动Decode
最常见的方式
1. `kubectl get secret testsecret -o yaml `
1. 然后手动把base64编码后的字符串复制出来
1. 再去执行`echo xx | base64 -D`

这种应该是大部分使用k8s的开发人员最常用的方式，  
缺点是中间有一段手动复制的操作，用上了鼠标，效率比较低，不够high level~

## 使用jsonpath

这个其实是今天发现的，用jsonpath可以一条命令里完成取secret的操作  
比如有下面的secret 
```
kind: Secret
apiVersion: v1
data:
  properties: ClJFRElTX0hPU1Q9ZGFhcy1yZWRpcy1iZD
  user.password: hcy1yZWy1yZWRpcyRpcy1iZDClJFRElTX0hPU1Q9ZGF
```

这个secret有两个值，
假如我们想取properties的信息,可以使用
```
kubectl get secrets/testsecret -o jsonpath="{.data.properties}" | base64 -D
```

jsonpath可以把对应需要的文本筛选出来，就很舒服

假如key里面已经有了一个字符`.`应该怎么办？
```
kubectl get secrets/testsecret -o jsonpath="{.data\.properties}" | base64 -D
```

关于jsonpath的语法，可以看[这里](https://kubernetes.io/docs/reference/kubectl/jsonpath/)


## 使用插件kubectl view-secret

kubectl view-secret是一个kubectl的插件，
安装之后可以直接通过kubectl view-secret命令看secret的内容，  
我身边很多同事用的这个插件，一劳永逸~

[view-secret地址](https://github.com/elsesiy/kubectl-view-secret)

## 安装Lens
Lens是一个k8s可视化工具，可视化工具嘛，鼠标点点就出来了。  
Lens对应查pod，查pod里的log什么的都挺方便的，懒人必备~

![view-secret地址](/img/2022/4.1.jpg)

---

综述，以上几种方式，我的推荐顺序是 3 > 4 = 2 > 1  
kubectl view-secret装好后方便一些

下班~！
