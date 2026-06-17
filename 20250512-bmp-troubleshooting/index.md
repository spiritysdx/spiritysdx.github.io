# BKE 与 BMP 部署排障流程


这篇整理 BKE/BMP 部署和运行阶段的常用排障流程。核心原则是先确认当前处于部署前、部署中还是部署后的集群运行阶段，再选择对应的 `bke`、`kubectl`、`docker` 或系统服务排查路径。

## 部署命令排障

执行 `bke` 开头的命令时，可以打开调试输出：

```shell
DEBUG=true bke 后续的具体命令
```

执行初始化：

```shell
bke init
```

`bke init` 会启动一个 k3s，用于承载 Kubernetes 部署过程中的控制资源。命令执行完成后，先导出 kubeconfig：

```shell
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

如果部署 YAML 写错，需要重新创建集群，可以先暂停并删除 `bc`：

```shell
kubectl edit bc -n bke-cluster bke-cluster --kubeconfig=/etc/rancher/k3s/k3s.yaml
kubectl get bc -A --kubeconfig=/etc/rancher/k3s/k3s.yaml
kubectl delete bc -n bke-cluster bke-cluster --kubeconfig=/etc/rancher/k3s/k3s.yaml
```

编辑时将 `pause` 改为 `true`，等待状态变化后再删除资源，之后即可重新 create 修正后的 YAML。

## 已部署集群排障

先看所有命名空间的 Pod 状态：

```shell
kubectl get pods -A
```

对异常 Pod 先 `describe`：

```shell
kubectl describe pod license-xq13d -n kube-system
```

如果镜像能正常 pull，再看日志：

```shell
kubectl logs license-xq13d -n kube-system
kubectl logs license-xq13d -c controller -n kube-system
kubectl logs license-xq13d -n kube-system --tail 100
kubectl logs license-xq13d -n kube-system -f
```

如果 Pod 很快被创建又删除，普通 `get pods` 可能看不到，需要实时观察：

```shell
kubectl get pods -n kube-system -w
kubectl get jobs -n kube-system
kubectl get events -n kube-system
kubectl get events -n kube-system -w
```

## 替换集群中的镜像版本

本地开发完成后，如果已经 `kubectl apply -f manifest.yaml`，需要替换集群中的镜像版本，可以先查 Deployment：

```shell
kubectl get deployments -n kube-system
```

编辑对应 Deployment：

```shell
kubectl edit deployment <deployment-name> -n kube-system
```

修改镜像后，观察 Pod 是否被重建：

```shell
kubectl get pods -n kube-system -w
```

如果没有按预期替换，可以删除旧 Pod，依赖 Deployment 自动拉起：

```shell
kubectl delete pod license-xq13d -n kube-system
```

## 本地调试 API 或 Controller

如果需要本地启动 `api` 或 `controller`，先把集群中对应 Deployment 的副本数改成 `0`，避免调度冲突：

```shell
kubectl edit deployment <deployment-name> -n kube-system
```

修改 `spec.replicas` 后观察：

```shell
kubectl get deployment <deployment-name> -n kube-system -w
kubectl get pods -n kube-system -l app=<app-label> -w
```

## 集群断电重启异常

master 节点先查 Node：

```shell
kubectl get nodes -o wide
```

如果 kubelet 异常，继续查容器：

```shell
docker ps -a | grep kubelet
docker logs kubelet --tail 100
```

典型错误：

```text
failed to run Kubelet: running with swap on is not supported, please disable swap!
```

说明机器重启后自动挂载了 swap，导致 kubelet 起不来。处理方式：

```shell
free -g
sed -i '/swapfile/d' /etc/fstab
echo "3" >/proc/sys/vm/drop_caches
swapoff -a
rm -f /swapfile
```

然后在所有 master 节点上停止 `mariadb` 容器：

```shell
docker ps -a | grep mariadb
docker stop <mariadb-container>
```

修改：

```shell
vi /var/mariadb/data/grastate.dat
```

将最后一行 `safe_to_bootstrap` 从 `0` 改成 `1`，再启动所有 `mariadb` 容器，随后删除并重建 license Pod：

```shell
kubectl get pods -n kube-system
```

## HTTP 仓库 400 Bad Request

纳管仓库是普通非高可用、HTTP 协议时，如果报 `400 Bad Request`，需要在部署机上修改 registry 配置：

```shell
vi /etc/docker/bocloud_image_registry/config.yml
```

注释 TLS 配置：

```yaml
# tls:
#   certificate: /etc/docker/registry/deploy.bocloud.k8s.crt
#   key: /etc/docker/registry/deploy.bocloud.k8s.key
```

重启 registry：

```shell
docker restart bocloud_image_registry
```

## 删除 Docker 代理

如果用户修改过 Docker 代理，集群更新镜像时可能被代理污染。检查：

```text
/etc/systemd/system/docker.service.d/http-proxy.conf
```

备份后删除该文件，重载 Docker，再删除失败 Pod 让它重建。

```shell
systemctl daemon-reload
systemctl restart docker
kubectl delete pod <pod-name> -n <namespace>
```

