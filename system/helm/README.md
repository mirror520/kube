# Helm

官方網站：[Helm](https://helm.sh/)

## 1. 安裝 Helm CLI

至 Helm Github 下載最新 Release Binary ( [Helm Release](https://github.com/helm/helm/releases) )

```bash
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.12.1-linux-amd64.tar.gz
tar -xvf helm-v2.12.1-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
```

## 2. RBAC

```bash
kubectl apply -f rbac-config.yaml
```

## 3. Helm 初始化

```bash
# Helm 初始化
helm init --service-account tiller

# 更新 Helm Repo
helm repo update

# Helm search
helm search

# 顯示 Helm 版本
# 看 Helm Server 及 Client 是否都有顯示出來
helm version
```

註：本地端也安裝 Helm Client，他會自己去讀 $HOME/.kube/config