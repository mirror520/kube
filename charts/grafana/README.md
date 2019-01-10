# Grafana

## 安裝

因為我沒有打算讓 Grafana 保存資料，
所以我在 Values.yaml 加入 Prometheus 資料來源，
記得需把 datasource 參數後的空 `{}` 刪除。

```bash
helm install stable/grafana --name=grafana --namespace=kube-system -f values.yaml
```

## 我目前用到的 Dashboard

Kubernetes: 8588
Ceph: 7056