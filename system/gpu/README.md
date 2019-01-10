# Schedule GPUs

相關網站：

- [CUDA](https://developer.nvidia.com/cuda-downloads)
- [Nvidia Docker](https://github.com/NVIDIA/nvidia-docker)
- [NVIDIA device plugin for Kubernetes](https://github.com/NVIDIA/k8s-device-plugin)
- [Kubernetes - Schedule GPUs](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/)

## 1. 安裝 GPU Driver 及 Nvidia CUDA

使用 `runfile`

```bash
wget https://developer.nvidia.com/compute/cuda/10.0/Prod/local_installers/cuda_10.0.130_410.48_linux

sudo apt install gcc make
sudo ./cuda_10.0.130_410.48_linux
```

加入環境變數

測試

```bash
# 測試 GPU 驅動 及 CUDA 版本
nvidia-smi

# nvcc
nvcc -V
```

註：下次重灌電腦再補詳細操作

## 2. 安裝 Docker Nvidia

```bash
# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
```

## 3. 安裝 NVIDIA device plugin for Kubernetes

變更 Docker 預設啟動 Nvidia Container

`sudo vi /etc/docker/daemon.json`

```json
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

```bash
kubectl apply -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.12/nvidia-device-plugin.yml
```

## 加入 Node Label

```bash
kubectl label nodes kube-worker-2 accelerator=nvidia-gtx-1060
```