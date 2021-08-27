# 学习 Istio

## 在线学习平台
katacoda.com

## 安装
在 katacoda 里面的 现成 k8s 环境里面
```bash
curl -L https://istio.io/downloadIstio | sh -
```
再按照提示
```bash
export PATH="$PATH:/root/istio-1.10.3/bin"
```

### 安装命令
默认安装
istioctl manifest apply 
istioctl manifest apply --set profile=demo # default demo minimal remote empty

## 部署 Bookinfo
### 首先必须要注入 SideCar
kubectl label namespace default iotio-injection=enabled --overwrite=true namespace/default labeled

### 部署应用
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

### 创建网关
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml