# Storage

相關網址：

- [External Storage](https://github.com/kubernetes-incubator/external-storage)

## 0. StorageClass

就我現在的理解，StorageClass 可在 Cluster 層級建立一個抽象儲存，
例如建一個 Ceph RBD 的 StorageClass，就可以把它視為是 Ceph RBD。

Persistent Volume Claims(PVC) 是 namespace 層級的，他可以掛載到 Pods 上，
可以`視需要`是先建立好 PVC 讓它永久存在，
當建立 PVC 時，PVC 可向 StorageClass 請求所需的 Persistent Volume(PV)，
就像是沒有用 StorageClass 前，手動用 Ceph CLI 創建 RBD Block Image，再用 PV、PVC 綁定，
StorageClass (Ceph) 會自動建立指定大小的 RBD Block Image (命名: pvc-xxxxxxxxx)，
再用 PV 去綁定該 Image，再用 PVC 綁定 PV。

PVC 刪除時，如果 PV 的資源回收政策設為刪除，也會自己在實體 Ceph 上刪除該 RBD Image。

## 1. 加入 External Provisioners

### 1.1. CephFS

我覺得 CephFS Provisioner 還沒製作好，在啟用RBAC情況下，Ceph Secret、Provisioner 似乎不能跨 namespace，不然會無法存取 Secret。
所以我用比較麻煩的方法，每個會用到 CephFS Provisioner 的 Namespace 皆建立一份。

```bash
kubectl apply -f ./external-storage/cephfs
```

### 1.2. Ceph RBD

RBD 的 External Provisioner 製作就比較完整，我覺得它應該就是 Kubernetes 內部那一個，
他沒有 CephFS 的問題，可以直接把 Provisioner 建在 namespaces/kube-system，把管理者金鑰放在 `kube-system`，把使用者金鑰放在 `default`。

```bash
kubectl apply -f ./external-storage/rbd
```

## 2. 加入 StorageClass

將 Ceph Auth Keys 匯入

```bash
kubectl apply -f ./storage-class/ceph-secret.yaml
```

### 2.1. CephFS

StorageClass 是 Cluster 層級的，但是我還不知道怎麼解決 CephFS Provisioner 跨 Namespace 的問題，所以各建一個 StorageClass

```bash
kubectl apply -f ./storage-class/cephfs-storageclass.yaml
```

### 2.2. Ceph RBD

```bash
kubectl apply -f ./storage-class/rbd-storageclass.yaml
```