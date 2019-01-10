# Kubernetes Dashboard

官方網站：[Kubernetes Dashboard](https://github.com/kubernetes/dashboard/)

## 1. 安裝

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```

## 2. RBAC

```bash
kubectl apply -f kubernetes-dashboard.yaml
```

## 3. 進入系統

本機電腦安裝 `kubectl`，並加入 $HOME/.kube/config 設定檔

用本機代理遠端 Kubernetes Cluster API

```bash
kubectl proxy
```

瀏覽器開啟

[http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/)

## 4. 登入方式

### 4.1 使用 Bearer Token

### 4.2 使用 Kube Config
