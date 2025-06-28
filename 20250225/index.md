# k8s日常问题排障


## 一般流程

```shell
free -m
```

需要确保无SWAP，否则`kubelet`起不来

然后需要

```shell
docker ps -a | grep etcd
```

看平面容器起来了没有，没有的话就得看容器日志排查问题

然后

```shell
kubectl get pods -A
```

看核心的namespace的pod有没有起来，有没有ready，有问题的pod名字就```describe```一下
下面所有示例都用```kube-system```作为查询的namespace，实际看你要查什么服务对应的namespace

```shell
kubectl describe pod license-xq13d -n kube-system
```

如果正常```pull```拉得到镜像，那就得查```log```了

```shell
kubectl logs license-xq13d -n kube-system
```

如果pod有多个容器类型，比如有```api```的，有```controller```的，那就得```-c```指定容器类型

```shell
kubectl logs license-xq13d -c controller -n kube-system
```

有时候报错是一长串，得```--tail 100```

```shell
kubectl logs license-xq13d -n kube-system --tail 100
```

有时候得实时追踪，得```-f```

```shell
kubectl logs license-xq13d -n kube-system -f
```

类似的，也有命令实时看pod的

```shell
kubectl get pods -n kube-system -w
```

pod有可能因为调度器问题马上拉起来又被删了，执行这些命令查询查不到任何东西
这时得查```job```

```shell
kubectl get jobs -n kube-system
```

得查```events```

```shell
kubectl get events -n kube-system
```

有时候因为状态改变太快了，```job```和```events```也得```-w```实时打印，操作后查看状态改变前后耗时以及出现了什么状态
有时候pod需要修改```deployment```的```pull```的镜像的版本(常见本地开发完备了要更新上集群测试)，项目的```manifest.yaml```已经```kubectl apply -f```了
这时候就需要

```shell
kubectl get deployments -n kube-system
```

查询需要修改的pod名字对应的```deployments```名字
然后```edit```

```shell
kubectl edit deployment <deployment-name> -n kube-system
```

这时候起来的修改器类似```vim```和```vi```，可用```/```查询，可用 上下查看查到的关键词，用```exit```退出编辑，用```:wq```写入修改和退出修改器
修改后使用命令：

```shell
kubectl get pods -n kube-system -w
```

查询对应的pod有没有被重建，有没有如期```ready```
如果未能如期起来和替换，那么手动```delete```掉pod，因为有设置```spec.replicas```在```deployment```中，所以会重建pod

```shell
kubectl delete pod license-xq13d -n kube-system
```

有时候需要本地开发，本地启动```api```和```controller```，那么就需要集群中设置对应的服务的pod的副本数量修改为0，避免调度冲突

```shell
kubectl edit deployment <deployment-name> -n kube-system
```

在编辑器中找到```spec.replicas```字段并修改数值，然后保存退出(```:wq```)。
修改后使用以下命令监控副本数量变化：

```shell
kubectl get deployment <deployment-name> -n kube-system -w
```

或

```shell
kubectl get pods -n kube-system -l app=<app-label> -w
```

还有查```node```状态

```shell
kuebctl get nodes
```

一般的```k8s```运维看```BUG```这些足够了，更多的就得知道底层代码逻辑去判断了。

