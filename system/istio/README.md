# Istio

官方網站：[Istio](https://istio.io/)

## 1. 安裝 Istio CLI

```bash
# 下載 Release Binary
curl -L https://git.io/getLatestIstio | sh -

sudo mv istio-1.0.5 /usr/local/istio-1.0.5
```

加入環境變數

`vi ~/.profile`

```bash
export $ISTIO_HOME = "/usr/local/istio-1.0.5"
export $PATH = "$ISTIO_HOME/bin:$PATH"
```

測試 Istio CLI
`istioctl version`

## 2. 安裝 Istio

需先安裝 Helm

```bash
cd $ISTIO_HOME
helm install install/kubernetes/helm/istio --name istio --namespace istio-system
```