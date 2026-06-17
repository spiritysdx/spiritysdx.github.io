# Ray 支持昇腾 NPU 的 KubeRay 对接记录


这篇记录 Ray 在 Kubernetes 中对接昇腾 NPU 的流程，包括 Ray 镜像制作、KubeRay Operator 安装、RayCluster 编排和资源验证。环境基于 aarch64，镜像中需要带入宿主机昇腾驱动运行所需文件。

## 检查 NPU 占用

部署前先确认 NPU 没有被其他进程占用：

```shell
npu-smi info
```

只有确认目标卡空闲，后续 Ray worker 才更容易稳定拉起。

## Ray 镜像制作

构建目录中需要提前放入宿主机驱动相关文件：

- `dcmi`
- `npu-smi`
- `common`
- `driver`
- `ascend_install.info`
- `vnpu.cfg`
- `version.info`

Dockerfile 示例：

```dockerfile
FROM docker.spiritlhl.news/rayproject/ray:2.40.0.160e35-py310-aarch64

USER root

RUN apt-get update && apt-get install -y \
    language-pack-zh-hans \
    fonts-noto-cjk \
    fonts-wqy-microhei \
    && locale-gen zh_CN.UTF-8 && \
    update-locale LANG=zh_CN.UTF-8

ENV LANG=zh_CN.UTF-8
ENV LANGUAGE=zh_CN:zh
ENV LC_ALL=zh_CN.UTF-8

RUN apt-get update && apt-get install -y \
    libxrender1 \
    libxtst6 \
    libxi6 \
    libxext6 \
    libfontconfig1 \
    libfreetype6 \
    libx11-6 \
    git \
    vim \
    nano \
    sudo \
    python3-pip

RUN pip install ray[client]==2.40.0 --index-url https://pypi.tuna.tsinghua.edu.cn/simple

ENV LD_LIBRARY_PATH=/usr/local/Ascend/driver/lib64/common:$LD_LIBRARY_PATH
ENV LD_LIBRARY_PATH=/usr/local/Ascend/driver/lib64/driver:$LD_LIBRARY_PATH

COPY dcmi /usr/local/dcmi
COPY npu-smi /usr/local/bin/npu-smi
COPY common /usr/local/Ascend/driver/lib64/common
COPY driver /usr/local/Ascend/driver/lib64/driver
COPY ascend_install.info /etc/ascend_install.info
COPY vnpu.cfg /etc/vnpu.cfg
COPY version.info /usr/local/Ascend/driver/version.info

RUN chmod +x /usr/local/bin/npu-smi

ENV USER_ID=0
ENV GROUP_ID=0
RUN sed -i '/app::/d' /etc/passwd

WORKDIR /home/ubuntu/
ENV HOME=/home/ubuntu/
```

`docker.spiritlhl.news` 只是镜像加速地址，去掉后对应原 Docker Hub 镜像地址。

构建并推送到内网仓库：

```shell
docker build -t deploy.bocloud.k8s:40443/public/ray:2.40.0-py310-aarch64-npu .
```

## 安装 KubeRay

```shell
git clone https://github.com/ray-project/kuberay.git
cd kuberay/ray-operator/config/default
```

确认当前目录存在 `kustomization.yaml` 后安装：

```shell
kubectl create -k .
kubectl get pods -n default
```

## 创建 RayCluster

创建 `raycluster.yaml`：

```yaml
apiVersion: ray.io/v1
kind: RayCluster
metadata:
  name: raycluster-npu
  namespace: default
spec:
  rayVersion: "2.40.0"
  headGroupSpec:
    serviceType: ClusterIP
    rayStartParams:
      dashboard-host: "0.0.0.0"
    template:
      spec:
        containers:
          - name: ray-head
            image: deploy.bocloud.k8s:40443/public/ray:2.40.0-py310-aarch64-npu
            imagePullPolicy: IfNotPresent
            env:
              - name: ASCEND_VISIBLE_DEVICES
                value: "0"
            ports:
              - containerPort: 6379
              - containerPort: 8265
              - containerPort: 10001
            volumeMounts:
              - name: dev-davinci0
                mountPath: /dev/davinci0
              - name: dev-davinci-manager
                mountPath: /dev/davinci_manager
              - name: dev-devmm
                mountPath: /dev/devmm_svm
              - name: dev-hdc
                mountPath: /dev/hisi_hdc
              - name: ascend-root
                mountPath: /usr/local/Ascend
            securityContext:
              privileged: true
        volumes:
          - name: dev-davinci0
            hostPath:
              path: /dev/davinci0
              type: CharDevice
          - name: dev-davinci-manager
            hostPath:
              path: /dev/davinci_manager
              type: CharDevice
          - name: dev-devmm
            hostPath:
              path: /dev/devmm_svm
              type: CharDevice
          - name: dev-hdc
            hostPath:
              path: /dev/hisi_hdc
              type: CharDevice
          - name: ascend-root
            hostPath:
              path: /usr/local/Ascend
              type: Directory
  workerGroupSpecs:
    - groupName: worker-group
      replicas: 2
      minReplicas: 1
      maxReplicas: 2
      rayStartParams: {}
      template:
        spec:
          containers:
            - name: ray-worker
              image: deploy.bocloud.k8s:40443/public/ray:2.40.0-py310-aarch64-npu
              imagePullPolicy: IfNotPresent
              env:
                - name: ASCEND_VISIBLE_DEVICES
                  value: "0"
              volumeMounts:
                - name: dev-davinci0
                  mountPath: /dev/davinci0
                - name: dev-davinci-manager
                  mountPath: /dev/davinci_manager
                - name: dev-devmm
                  mountPath: /dev/devmm_svm
                - name: dev-hdc
                  mountPath: /dev/hisi_hdc
                - name: ascend-root
                  mountPath: /usr/local/Ascend
                - name: dcmi-root
                  mountPath: /usr/local/dcmi
              securityContext:
                privileged: true
          volumes:
            - name: dev-davinci0
              hostPath:
                path: /dev/davinci0
                type: CharDevice
            - name: dev-davinci-manager
              hostPath:
                path: /dev/davinci_manager
                type: CharDevice
            - name: dev-devmm
              hostPath:
                path: /dev/devmm_svm
                type: CharDevice
            - name: dev-hdc
              hostPath:
                path: /dev/hisi_hdc
                type: CharDevice
            - name: ascend-root
              hostPath:
                path: /usr/local/Ascend
                type: Directory
            - name: dcmi-root
              hostPath:
                path: /usr/local/dcmi
                type: Directory
```

应用配置：

```shell
kubectl apply -f raycluster.yaml
kubectl get rayclusters
kubectl get pods -n default -l ray.io/cluster=raycluster-npu
```

## 验证 Ray 集群资源

参考 Ray 官方 Kubernetes 快速开始文档：

<https://docs.ray.io/en/latest/cluster/kubernetes/getting-started/raycluster-quick-start.html>

获取 head Pod：

```shell
export HEAD_POD=$(kubectl get pods --selector=ray.io/node-type=head -o custom-columns=POD:metadata.name --no-headers)
echo "$HEAD_POD"
```

打印集群资源：

```shell
kubectl exec -it "$HEAD_POD" -- python -c "import pprint; import ray; ray.init(); pprint.pprint(ray.cluster_resources(), sort_dicts=True)"
```

期望能看到类似资源：

```text
{'CPU': 96.0,
 'NPU': 2.0,
 'memory': 84300556493.0,
 'node:10.250.0.45': 1.0,
 'node:__internal_head__': 1.0,
 'object_store_memory': 40414524211.0}
```

如果没有 `NPU`，优先检查设备挂载、`ASCEND_VISIBLE_DEVICES`、宿主机 `/usr/local/Ascend` 和 `/usr/local/dcmi` 是否一致。

## 清理 Ray 集群

```shell
kubectl delete -f raycluster.yaml
```

