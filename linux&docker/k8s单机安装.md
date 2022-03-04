#k8s 单机安装

## 参考资料

[Kubernetes 指南](https://feisky.gitbooks.io/kubernetes/content/)
[Kubernetes user-guide](http://kubernetes.kansea.com/docs/user-guide/kubectl/kubectl_proxy/)
[Kubernetes 指南 4. 部署配置 – 4.1 单机部署 ](https://www.wenjiangs.com/doc/t7gw2iehg)
[minikube Getting Started Guide](https://minikube.sigs.k8s.io/docs/start/)
[kubernetes 参考文档](http://kubernetes.kansea.com/docs/user-guide/kubectl/kubectl_proxy/)

[Centos7.6部署k8s(v1.14.2)集群](https://blog.51cto.com/loong576/2405624)

[minikube doc](https://minikube.sigs.k8s.io/docs/)

## 操作步骤

```sh
[root@dev002 k8s]# curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
[root@dev002 k8s]# sudo install minikube-darwin-amd64 /usr/local/bin/minikube
[root@dev002 k8s]# minikube start
😄  Centos 7.7.1908 上的 minikube v1.25.1
✨  自动选择 docker 驱动。其他选项：none, ssh
🛑  The "docker" driver should not be used with root privileges.
💡  If you are running minikube within a VM, consider using --driver=none:
📘    https://minikube.sigs.k8s.io/docs/reference/drivers/none/

❌  Exiting due to DRV_AS_ROOT: The "docker" driver should not be used with root privileges.

如果出现了上面的问题增加 --force --driver=docker 参数。

[root@dev002 k8s]# minikube start --force --driver=docker
😄  Centos 7.7.1908 上的 minikube v1.25.1
❗  minikube skips various validations when --force is supplied; this may lead to unexpected behavior
✨  根据用户配置使用 docker 驱动程序
🛑  The "docker" driver should not be used with root privileges.
💡  If you are running minikube within a VM, consider using --driver=none:
📘    https://minikube.sigs.k8s.io/docs/reference/drivers/none/
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.23.1 preload ...
    > preloaded-images-k8s-v16-v1...: 504.42 MiB / 504.42 MiB  100.00% 10.22 Mi
    > index.docker.io/kicbase/sta...: 378.98 MiB / 378.98 MiB  100.00% 1.83 MiB
❗  minikube was unable to download gcr.io/k8s-minikube/kicbase:v0.0.29, but successfully downloaded docker.io/kicbase/stable:v0.0.29 as a fallback image
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
❗  This container is having trouble accessing https://k8s.gcr.io
💡  To pull new external images, you may need to configure a proxy: https://minikube.sigs.k8s.io/docs/reference/networking/proxy/
🐳  正在 Docker 20.10.12 中准备 Kubernetes v1.23.1…
    ▪ kubelet.housekeeping-interval=5m
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default


[root@dev002 k8s]# minikube kubectl -- get po -A
    > kubectl.sha256: 64 B / 64 B [--------------------------] 100.00% ? p/s 0s
    > kubectl: 44.43 MiB / 44.43 MiB [-------------] 100.00% 10.05 MiB p/s 4.6s
NAMESPACE     NAME                               READY   STATUS    RESTARTS       AGE
kube-system   coredns-64897985d-gchvs            1/1     Running   0              2m10s
kube-system   etcd-minikube                      1/1     Running   0              2m24s
kube-system   kube-apiserver-minikube            1/1     Running   0              2m20s
kube-system   kube-controller-manager-minikube   1/1     Running   0              2m25s
kube-system   kube-proxy-qt284                   1/1     Running   0              2m10s
kube-system   kube-scheduler-minikube            1/1     Running   0              2m24s
kube-system   storage-provisioner                1/1     Running   2 (2m6s ago)   2m18s

[root@dev002 k8s]# alias kubectl="minikube kubectl --"
[root@dev002 k8s]# minikube dashboard
🔌  正在开启 dashboard ...
    ▪ Using image kubernetesui/dashboard:v2.3.1
    ▪ Using image kubernetesui/metrics-scraper:v1.0.7
🤔  正在验证 dashboard 运行情况 ...
🚀  Launching proxy ...
🤔  正在验证 proxy 运行状况 ...
http://127.0.0.1:44725/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/

```

上面的地址是未对外暴露的，解决如下：
```
首先执行 alias kubectl="minikube kubectl --"
然后在执行 kubectl ，验证kubectl命令是否已经存在。
其次，使用proxy代理到虚拟机的指定端口
kubectl proxy --port=8601 --address='192.168.0.202' --accept-hosts='^.*' &
启动后宿主机访问链接：
http://192.168.0.202:8601/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
```


在k8s重启后，导致docker出现“ERROR: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?”问题。
出现此问题重启docker即可。


# Helm 

[Using Helm](https://helm.sh/zh/docs/)
[Helm应用包管理](https://www.jianshu.com/p/25ca410b1efd)
[helm 基本使用](https://www.jianshu.com/p/b6ebf8fc72f7)
[helm使用](https://www.jianshu.com/p/6edc5ede7177)
[Helm example](https://github.com/bitnami/charts/tree/master/bitnami)
[nacos-k8s](https://github.com/nacos-group/nacos-k8s)
[Helm User Guide - Helm 用户指南](https://whmzsu.github.io/helm-doc-zh-cn/quickstart)
## 安装

```
[root@dev002 k8s]# wget https://get.helm.sh/helm-v3.4.2-linux-amd64.tar.gz
--2022-02-17 16:41:51--  https://get.helm.sh/helm-v3.4.2-linux-amd64.tar.gz
正在解析主机 get.helm.sh (get.helm.sh)... 152.199.39.108, 2606:2800:247:1cb7:261b:1f9c:2074:3c
正在连接 get.helm.sh (get.helm.sh)|152.199.39.108|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：13317454 (13M) [application/x-tar]
正在保存至: “helm-v3.4.2-linux-amd64.tar.gz”

100%[========================================================================================================>] 13,317,454   998KB/s 用时 20s    

2022-02-17 16:42:12 (661 KB/s) - 已保存 “helm-v3.4.2-linux-amd64.tar.gz” [13317454/13317454])

[root@dev002 k8s]# ls
helm-v3.4.2-linux-amd64.tar.gz  kubectl  minikube-linux-amd64
[root@dev002 k8s]# tar -zxvf helm-v3.4.2-linux-amd64.tar.gz
linux-amd64/
linux-amd64/README.md
linux-amd64/LICENSE
linux-amd64/helm

[root@dev002 k8s]# ls
helm-v3.4.2-linux-amd64.tar.gz  kubectl  linux-amd64  minikube-linux-amd64  

[root@dev002 k8s]# mv linux-amd64/helm /usr/bin/

[root@dev002 k8s]# ls /usr/bin/helm 
/usr/bin/helm

[root@dev002 k8s]# helm create mychart
Creating mychart

[root@dev002 k8s]# cd mychart/

[root@dev002 mychart]# ls
charts  Chart.yaml  templates  values.yaml
[root@dev002 mychart]# vi values.yaml 

[root@dev002 mychart]# cd ..

[root@dev002 k8s]# ls
helm-v3.4.2-linux-amd64.tar.gz  kubectl  linux-amd64  minikube-linux-amd64  mychart


[root@dev002 k8s]# helm install web mychart --set service.type=NodePort --set persistence.enabled=false
NAME: web
LAST DEPLOYED: Fri Feb 18 16:14:56 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services web-mychart)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT


[root@dev002 k8s]# helm list
NAME	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART        	APP VERSION
web 	default  	1       	2022-02-17 16:46:13.941345918 +0800 CST	deployed	mychart-0.1.0	1.16.0     

[root@dev002 k8s]# helm status web
NAME: web
LAST DEPLOYED: Fri Feb 18 16:14:56 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services web-mychart)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT


[root@dev002 k8s]# kubectl get pod,svc
NAME                               READY   STATUS    RESTARTS   AGE
pod/web-mychart-5dfbf6c544-f8qgb   1/1     Running   0          6m44s

NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP   120m
service/web-mychart   ClusterIP   10.98.124.152   <none>        80/TCP    6m44s

```

helm install web mynginx --set service.type=NodePort --set persistence.enabled=false

## helm文件创建



## 问题

Q1: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running
此问题可能是docker服务未启动。可以通过 systemctl status docker  查看服务状态。若未启动使用下面指令启动：
systemctl start docker

Q2: 机器重启后k8s未启动 ，服务启动顺序
1. systemctl start docker
2. minikube start --force --driver=docker
3. alias kubectl="minikube kubectl --"
4. minikube dashboard
