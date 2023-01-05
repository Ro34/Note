### YAML格式
```shell
error: error parsing deployment.yaml: error converting YAML to JSON: yaml: line 9: could not find expected ':'
```
> 需要在遇到：或者- 增加一个空格

### 拉取镜像
1. image不需要添加tag
2. Failed to pull image "mw": rpc error: code = Unknown desc = Error response from daemon: pull access denied for mw, repository does not exist or may require 'docker login': denied: requested access to the resource is denied

### 上传镜像
http: server gave HTTP response to HTTPS client
> vim /usr/lib/systemd/system/docker.service
> 在execstart后面添加 --insecure-registry ip：5000
>
initialization failed, will try again: wait: /bin/bash -c "sudo env PATH="/var/lib/minikube/binaries/v1.25.3:$PATH" kubeadm init --config /var/tmp/minikube/kubeadm.yaml  --ignore-preflight-errors=DirAvailable--etc-kubernetes-manifests,DirAvailable--var-lib-minikube,DirAvailable--var-lib-minikube-etcd,FileAvailable--etc-kubernetes-manifests-kube-scheduler.yaml,FileAvailable--etc-kubernetes-manifests-kube-apiserver.yaml,FileAvailable--etc-kubernetes-manifests-kube-controller-manager.yaml,FileAvailable--etc-kubernetes-manifests-etcd.yaml,Port-10250,Swap,Mem,SystemVerification,FileContent--proc-sys-net-bridge-bridge-nf-call-iptables": Process exited with status 1

### CNI
```
cni plugin not initialized
```


Get "https://172.18.60.235:6443/api/v1/nodes?limit=500": dial tcp 172.18.60.235:6443: connect: connection refused - error from a previous attempt: http2: server sent GOAWAY and closed the connection; LastStreamID=51, ErrCode=NO_ERROR, debug=""

### healthy
```
It seems like the kubelet isn't running or healthy.
```
