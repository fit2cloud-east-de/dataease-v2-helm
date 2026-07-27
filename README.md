# Helm Chart 部署 DataEase

DataEase Kubernetes Helm Chart（分支 `v2.10.20`）。

编排已对齐官方 installer：`Playwright` 截图、`APISIX`（含 hmac-auth）、`map` 数据卷。  
安装包 / 本仓库仅使用统一的 `values.yaml`（通过 `DataEase.engine_mode` 切换社区版/企业版），**不使用** `values-enterprise.yaml`。

## 1. 获取 Chart

```bash
git clone -b v2.10.20 https://github.com/fit2cloud-east-de/dataease-v2-helm.git
cd dataease-v2-helm
```

或从 [Releases](https://github.com/fit2cloud-east-de/dataease-v2-helm/releases/tag/v2.10.20) 下载社区版/企业版 zip。

## 2. 配置存储类

```yaml
common:
  storageClass: nfs-sc
```

## 3. 选择安装模式

在 `values.yaml` 中设置：

```yaml
DataEase:
  engine_mode: community   # 或 enterprise
```

企业版会额外部署 APISIX、Playwright、同步任务等组件。

## 4. 安装

```bash
kubectl create ns de2
helm install dataease-v2 . -n de2
kubectl get pod -n de2
kubectl logs -f dataease -n de2
```

## 5. 升级

```bash
helm upgrade dataease-v2 . -n de2
```

## 6. 访问

```text
社区版 NodePort: http://<节点IP>:30082
企业版 NodePort: http://<节点IP>:30090
Ingress: http://demo.apps.dataease.com

用户名: admin
密码: DataEase@123456
```

镜像对齐官方 installer（`v2.10.20`）：
- dataease / dataease-sync-task: `v2.10.20`
- de-playwright-api: 见 `values.yaml`
- apisix: 见 `values.yaml`