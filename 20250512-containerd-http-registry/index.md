# containerd 允许 HTTP 镜像仓库拉取


这篇记录 `containerd` 拉取 HTTP 镜像仓库时的配置方式。常见误区是只在 `/etc/containerd/config.toml` 中配置 `insecure = true`，但这个配置主要作用于 CRI 插件；使用 `nerdctl`、`ctr` 或 Kubernetes 拉取时，仍可能继续尝试 HTTPS。

## 问题现象

私有仓库只提供 HTTP 服务时，直接拉取可能出现类似问题：

- `server gave HTTP response to HTTPS client`
- `http: server gave HTTP response to HTTPS client`
- Kubernetes Pod 拉取镜像失败，但 `config.toml` 中已经设置过 `insecure = true`

根因是 `containerd` 新版本推荐通过 `certs.d` 目录为每个 registry 显式配置 `hosts.toml`。

## 临时拉取

使用 `ctr` 时加 `--plain-http`：

```shell
ctr image pull --plain-http 仓库地址/镜像名:标签
```

使用 `nerdctl` 时加 `--insecure-registry`：

```shell
nerdctl -n k8s.io pull --insecure-registry 仓库地址/镜像名:标签
```

临时参数适合排障确认，不适合长期运行环境。

## 持久化配置

以 `deploy.bocloud.k8s:40443` 为例，创建证书配置目录：

```shell
mkdir -p /etc/containerd/certs.d/deploy.bocloud.k8s:40443
```

写入 `hosts.toml`：

```shell
vim /etc/containerd/certs.d/deploy.bocloud.k8s:40443/hosts.toml
```

内容如下：

```toml
server = "deploy.bocloud.k8s:40443"

[host."http://deploy.bocloud.k8s:40443"]
  skip_verify = true
  plain_http = true
```

重启 `containerd`：

```shell
systemctl daemon-reload
systemctl restart containerd
```

## 验证

使用 Kubernetes 命名空间对应的镜像仓库上下文测试：

```shell
nerdctl -n k8s.io pull deploy.bocloud.k8s:40443/命名空间/镜像名:标签
```

如果拉取成功，再创建 Pod 或重新删除失败 Pod，让 kubelet 重新触发镜像拉取。

## 注意事项

- `hosts.toml` 的目录名必须与镜像地址中的 registry 完全一致，包括端口。
- `plain_http = true` 是允许 HTTP 的关键配置。
- `skip_verify = true` 只是不校验证书，不能单独让 HTTPS 降级为 HTTP。
- 生产环境能使用 HTTPS 时仍应优先使用 HTTPS。

