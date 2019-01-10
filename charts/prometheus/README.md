# Prometheus 安裝

## 先產生 Prometheus Server 及其 AlertManager 所需的 Volumes

我有修改 Values.yaml 裡的 Prometheus Server 及 AlertManager的 PVC 設定，
設定 existingClaim 及 storageClass 設為 `-`，讓它不會自動產生 PVC，並且能持續保留資料。

```bash
helm install stable/prometheus --name=prometheus --namespace kube-system -f values.yaml
```

## Ceph Metrics

Prometheus 的設定檔也可以在 Values.yaml 建立，
我把 Ceph Metrics 設定增加進 extraScrapeConfigs，
該參數是存文字段落，需在 `:` 後加入 `|`
