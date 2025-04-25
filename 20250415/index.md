# kubevirt初体验


## 确认当前k8s版本以及对应的kubevirt版本

```shell
kubectl version
```

显示

```
Client Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.17", GitCommit:"953be8927218ec8067e1af2641e540238ffd7576", GitTreeState:"clean", BuildDate:"2023-02-22T13:34:27Z", GoVersion:"go1.19.6", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.17", GitCommit:"953be8927218ec8067e1af2641e540238ffd7576", GitTreeState:"clean", BuildDate:"2023-02-22T13:27:46Z", GoVersion:"go1.19.6", Compiler:"gc", Platform:"linux/amd64"}
```

根据

https://github.com/kubevirt/sig-release/commits/main/releases/k8s-support-matrix.md

查询历史支持的kubevirt版本，得知支持的最新的是kubevirt 0.58

## 下载安装

由于在境内，github不畅通，使用```https://cdn.spiritlhl.net/```代理下载相关文件

### **1. 部署 KubeVirt Operator**
```sh
export VERSION=v0.58.1
kubectl create -f https://cdn.spiritlhl.net/https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/kubevirt-operator.yaml
```

### **2. 创建 KubeVirt CR**
```sh
kubectl create -f https://cdn.spiritlhl.net/https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/kubevirt-cr.yaml
```

### **3. 确认安装**
```sh
kubectl get pods -n kubevirt
```
等待所有 Pod 进入 `Running` 或 `Completed` 状态。(实测大致需要5~10分支，拉取```quay.io/kubevirt/```的镜像，好在这个域名没被ban)

```sh
kubectl get kubevirt -n kubevirt
```
如果 `Phase` 显示 `Deployed`，说明 KubeVirt 已成功安装。  

---

### **4. 安装 `virtctl`**
`virtctl` 是管理虚拟机的命令行工具：
```sh
curl -L -o virtctl https://cdn.spiritlhl.net/https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/virtctl-${VERSION}-linux-amd64
chmod +x virtctl
sudo mv virtctl /usr/local/bin/
```
你可以用 `virtctl` 创建、管理 Windows Server Core 虚拟机。  

## 测试开设KVM虚拟机

检测集群是否支持KVM

```shell
kubectl get nodes -o custom-columns="NAME:.metadata.name,CPU Manager:.status.allocatable.cpu,Device Plugin:.status.allocatable.kubevirt\.io/kvm"
```

虚拟机的yaml

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test
---
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  name: cirros
  namespace: test
spec:
  running: true
  template:
    metadata:
      labels:
        kubevirt.io/size: small
        kubevirt.io/domain: cirros
    spec:
      domain:
        devices:
          disks:
            - name: containerdisk
              disk:
                bus: virtio
            - name: cloudinitdisk
              disk:
                bus: virtio
          interfaces:
            - name: default
              masquerade: {}
        resources:
          requests:
            memory: 128Mi  # **改成 128Mi，避免 64M 太小导致 KVM 启动失败**
      networks:
        - name: default
          pod: {}
      volumes:
        - name: containerdisk
          containerDisk:
            image: quay.io/kubevirt/cirros-container-disk-demo
        - name: cloudinitdisk
          cloudInitNoCloud:
            userData: |
              #cloud-config
              password: "cirros"
              chpasswd: { expire: False }
```

创建虚拟机

```shell
kubectl apply -f cirros-vm.yaml
```

检测虚拟机是否已启动，如果 PHASE 是 Running，说明虚拟机已成功启动。

```shell
kubectl get vmi -n test
```

连接 Cirros 虚拟机

```shell
virtctl console cirros -n test
```

默认用户名：```cirros```
密码：```gocubsgo```

## 启动一个VNC服务(可选)

如果虚拟机本身没有图形界面，就没必要构建

```shell
kubectl apply -f virtvnc.yaml
```

yaml内容如下

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: virtvnc
  namespace: kubevirt
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: virtvnc
subjects:
- kind: ServiceAccount
  name: virtvnc
  namespace: kubevirt
roleRef:
  kind: ClusterRole
  name: virtvnc
  apiGroup: rbac.authorization.k8s.io
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: virtvnc
rules:
- apiGroups:
  - subresources.kubevirt.io
  resources:
  - virtualmachineinstances/console
  - virtualmachineinstances/vnc
  verbs:
  - get
- apiGroups:
  - kubevirt.io
  resources:
  - virtualmachines
  - virtualmachineinstances
  - virtualmachineinstancepresets
  - virtualmachineinstancereplicasets
  - virtualmachineinstancemigrations
  verbs:
  - get
  - list
  - watch
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: virtvnc
  name: virtvnc
  namespace: kubevirt
spec:
  ports:
  - port: 8001
    protocol: TCP
    targetPort: 8001
  selector:
    app: virtvnc
  type: NodePort
---
apiVersion: apps/v1  # 修正这里
kind: Deployment
metadata:
  name: virtvnc
  namespace: kubevirt
spec:
  replicas: 1
  selector:  # 新增 selector，必须匹配 template 里的 labels
    matchLabels:
      app: virtvnc
  template:
    metadata:
      labels:
        app: virtvnc
    spec:
      serviceAccountName: virtvnc
      nodeSelector:
        node-role.kubernetes.io/master: ""
      tolerations:
      - key: "node-role.kubernetes.io/master"
        operator: "Equal"
        value: ""
        effect: "NoSchedule"
      containers:
      - name: virtvnc
        image: quay.io/samblade/virtvnc:v0.1
        livenessProbe:
          httpGet:
            port: 8001
            path: /
            scheme: HTTP
          failureThreshold: 30
          initialDelaySeconds: 30
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 5
```

部署后检测

```shell
kubectl get pods -n kubevirt
kubectl get svc -n kubevirt
```

可见

```
[root@master-167 kubevirt]# kubectl get pods -n kubevirt
NAME                               READY   STATUS    RESTARTS       AGE
virt-api-5557bb6fdb-r87ht          1/1     Running   0              171m
virt-controller-79f97cfbbd-2gl8c   1/1     Running   1 (144m ago)   170m
virt-controller-79f97cfbbd-9tkdw   1/1     Running   4 (148m ago)   170m
virt-handler-hhssq                 1/1     Running   0              170m
virt-operator-7c9fd57795-7rr2m     1/1     Running   2 (144m ago)   174m
virt-operator-7c9fd57795-8655v     1/1     Running   3 (148m ago)   174m
virtvnc-548df7b7cd-7nlp4           1/1     Running   0              110s
[root@master-167 kubevirt]# kubectl get svc -n kubevirt
NAME                          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubevirt-operator-webhook     ClusterIP   10.96.227.221   <none>        443/TCP          172m
kubevirt-prometheus-metrics   ClusterIP   10.96.191.28    <none>        443/TCP          172m
virt-api                      ClusterIP   10.96.57.55     <none>        443/TCP          172m
virt-exportproxy              ClusterIP   10.96.150.110   <none>        443/TCP          172m
virtvnc                       NodePort    10.96.123.159   <none>        8001:30091/TCP   118s
```

## 后记

跑起来流程大概就是

qcow2 --> 加载创建PVC --> 创建网络和crd --> pod启动对应的容器 --> VNC捕获页面
