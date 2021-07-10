# pulsar 使用

## 环境准备
centos7

安装 podman 
```
# 先获取软件源
curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/CentOS_7/devel:kubic:libcontainers:stable.repo
# 再实际安装
yum install podman -y
# 释放内存
echo 3 > /proc/sys/vm/drop_caches
```

### 安装 pulsar 相关的 images
```

podman pull apachepulsar/pulsar
podman pull apachepulsar/pulsar-dashboard
podman pull apachepulsar/pulsar-all

```

### 部署
``` 
# 先运行单机版的 standalone 
# 先创建基础数据目录 /root/workspace/pulsar-proj/test/data
# 运行命令
podman run --name myPulsar -dit -p 18080:8080 -p 16650:6650 -v /root/workspace/pulsar-proj/test/data:/pulsar/data apachepulsar/pulsar bin/pulsar standalone
# 注意此处 --link 会出错
podman run --name myPulsar-dashboard -dit -p 8081:80 -e SERVICE_URL=http://pulsar:18080 --link myPulsar apachepulsar/pulsar-dashboard
# 先把 --link 去掉
podman run --name myPulsar-dashboard -dit -p 8081:80 -e SERVICE_URL=http://pulsar:18080 apachepulsar/pulsar-dashboard

# 测试
podman exec -it myPulsar bash bin/pulsar-client produce my-topic --messages "hello-pulsar"

```

### 获取 运行中podman container 内部文件的 方法
比如此 container 运行的id可以使用 xyz 标识，其内部有 /pulsar/conf 文件夹，想把其 /pulsar/conf 文件夹复制到宿主机上当前目录里可以使用如下命令：
```
podman cp xyz:/pulsar/conf .
```



### 结合 pod 
```
# 需要注意，国内的服务器不能访问 k8s.gcr.io 
# 所以要使用特殊手段处理
# 先使用国内的镜像访问到 pause:3.1 （目前访问3.2失败）
podman pull registry.aliyuncs.com/mirrorgooglecontainers/pause:3.1
# 再打 tag 为 k8s.gcr.io/pause:3.2
podman tag registry.aliyuncs.com/mirrorgooglecontainers/pause:3.1 k8s.gcr.io/pause:3.2
# 如此才能进行下一步
# 先创建 pod
podman pod create -n myPulsar-pod -p 18080:8080 -p 16650:6650 -p 1080:80
# 再把 pulsar 服务绑定到 那个 pod 上
podman run --name myPulsar -dit --pod myPulsar-pod -v /root/workspace/pulsar-proj/test/data:/pulsar/data apachepulsar/pulsar bin/pulsar standalone

# 再把 pulsar-dashboard 服务绑定到 那个 pod 上
podman run --name myPulsar-dashboard -dit --pod myPulsar-pod -e SERVICE_URL=http://0.0.0.0:8080 apachepulsar/pulsar-dashboard

```

### 结合 YAML 文件创建 pod
```
# my-pulsar-app.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pulsar-app
spec:
  containers:
  - name: my-pulsar
    image: apachepulsar/pulsar
    ports:
      - containerPort: 8080
        hostPort: 18080
        protocol: TCP
      - containerPort: 6650
        hostPort: 16650
        protocol: TCP
    volumeMounts:
      - name: my-pulsar-volume
        mountPath: /pulsar/data
    command: ['bin/pulsar']
    args: ["standalone"]
  - name: my-pulsar-dashboard
    image: apachepulsar/pulsar-dashboard
    ports:
      - containerPort: 80
        hostPort: 1080
        protocol: TCP
    env:
    - name: SERVICE_URL
      value: "http://0.0.0.0:8080"

  volumes:
    - name: my-pulsar-volume
      hostPath:
        path: /root/workspace/pulsar-proj/test/data
        type: Directory


```
### 处理 yaml 文件，并运行等操作
```
# 运行
podman play kube ./my-pulsar-app.yaml

# 查看 pod 状态
podman pod ps

# 停止
podman pod stop my-pulsar-app

# 启动
podman pod start my-pulsar-app

# 删除 （要先停止）
podman pod rm my-pulsar-app

```


