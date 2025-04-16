---
title: k8s端口转发脚本
description: 分享一个自用的k8s端口转发脚本
date: 2022-07-22 08:00:00+0800
categories: ["编程", "k8s"]
tags: ["k8s"]
weight: 5
---

## k8s端口转发原理


我们知道有一些数据库或者服务的API设有白名单机制, 只有在特定的生产集群内部才能访问。  
因为不对公网暴露，我们本地肯定是访问不了的。

而在日常工作嘛，总会有一些紧急需求需要连上这些服务，在本地进行一些调试、触发、执行SQL什么的临时性操作，
那这时候就需要把对应的端口转发出来。

我们知道`kubectl port-forward`可以把k8s的`service`转发到你的本地，  
因此一个常见的做法就是在k8s上创建一个pod专门作为转接，由这个Pod去访问目标端口，  
同时通过`port-forward`把对应的pod和端口转到你本地，这样就可以在本地进行连接调试了。

下面发一下我日常使用的脚本：

```shell
set -e

# args
KubeCfg=test--prod # 目标集群的kubeconfig -- kubecm的列表项
NameSpace=test-ns # 目标ns
REMOTE_HOST=test.remotehost.com # 需要转发的目标host
REMOTE_PORT=5432 # 需要转发的目标port
LOCAL_PORT=5439 # 本地port
TEMP_POD_NAME=test-portjump # 用于转发的pod名称

kubecm switch $KubeCfg

# 脚本退出时自动清理掉pod
function cleanup {
  echo "cleanup..." 
  kubectl delete -n $NameSpace pod/$TEMP_POD_NAME --grace-period 1 --wait=false
}
trap cleanup EXIT

kubectl run -n $NameSpace --image marcnuri/port-forward $TEMP_POD_NAME \
    --env REMOTE_HOST=$REMOTE_HOST \
    --env REMOTE_PORT=$REMOTE_PORT \
    --env LOCAL_PORT=$REMOTE_PORT \
    --port $REMOTE_PORT \
    --restart=Never 


kubectl wait -n $NameSpace --for=condition=Ready pod/$TEMP_POD_NAME
kubectl port-forward -n $NameSpace pod/$TEMP_POD_NAME $LOCAL_PORT:$REMOTE_PORT &
while true ; do sleep 60 ; nc -vz 127.0.0.1 $LOCAL_PORT ; done # 保持连接

```


两个注意点：

- 这个脚本需要安装一个`kubecm`，这是一个常用的切`kubeconfig`的工具  
- 脚本的最后写了一个循环，是因为kubectl port-forward这个命令在5min中没有操作的话会自动断开连接。  
实际工作中你可能出去倒杯咖啡就断了。因此需要需要这个循环来保持连接。  
(这个东西我翻了很多资料，这应该是client这边最好的保持连接的方式了)
不要滥用这个脚本。  
理论上这个脚本可以让你连上生产环境的一切地址~

一定要小心呐。一定要小心呐。一定要小心呐。