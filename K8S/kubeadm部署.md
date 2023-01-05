## 通过 kubeadm 在 ubuntu 上安装 kubenetes

### 前期准备
#### 关闭防火墙
```shell
sudo ufw disable
```

#### 安装docker
```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
# 或者
curl -sSL https://get.daocloud.io/docker | sh

# 验证版本
docker --version
```

#### 安装bridge-utils
> 用来操作linux bridge
```shell
sudo apt-get install bridge-utils
```

#### 设置iptables
要确保 br_netfilter 模块已经加载,可以通过运行 lsmod | grep br_netfilter来完成。
```shell
$ lsmod | grep br_netfilter

br_netfilter           32768  0
bridge                307200  1 br_netfilter
```
为了让作为Linux节点的iptables能看到桥接流量，应该确保 net.bridge.bridge-nf-call-iptables 在 sysctl 配置中被设置为1，执行命令：
```shell
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```
#### 禁用虚拟内存swap
执行 ``free -m`` 命令检测:
```shell
$ free -m        
              total        used        free      shared  buff/cache   available
Mem:          15896        1665       11376          20        2854       13819
Swap:             0           0           0
```
如果Swap这一行不是0，则说明虚拟内存swap被开启了，需要关闭。
###### 关闭方法
```shell
swapoff -a # 临时关闭
# 通过 sudo vi /etc/fstab 找到swap 这一行：
sed -ri 's/.*swap.*/#&/' /etc/fstab  #永久关闭
```
#### 设置docker的cgroup driver
docker 默认的 ``cgroup driver`` 是 ``cgroupfs``，可以通过 ``docker info`` 命令查看：
而 kubernetes 在v1.22版本之后，如果用户没有在 ``KubeletConfiguration`` 下设置 ``cgroupDriver`` 字段，则 kubeadm 将默认为 ``systemd``

需要修改 docker 的 ``cgroup driver`` 为 ``systemd``, 方式为打开 docker 的配置文件（如果不存在则创建）
```shell
sudo vi /etc/docker/daemon.json

# 增加内容
{
"exec-opts": ["native.cgroupdriver=systemd"]
}

# 重启docker
systemctl restart docker

# 重启后检查一下
docker info | grep "Cgroup Driver"
```
### 安装kubeadm
#### 官方方案
```shell
sudo -i
apt-get update
apt-get install -y apt-transport-https ca-certificates curl
curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

apt-get update
apt-get install -y kubelet kubeadm kubectl
```
#### 通过国内镜像源安装
```shell
# for Debian/Ubuntu configure ali mirror of k8s
apt-get update && apt-get install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
cat << EOF > /etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF
apt-get update
apt-get install  kubelet kubeadm kubectl
```

### 安装K8S
#### 官方方案
```shell
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 -v=9
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address={HostIP} -v=9

e.g.
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=172.18.60.235 -v=9

kubeadm init \
  --apiserver-advertise-address=172.18.60.135  \
  --image-repository registry.aliyuncs.com/google_containers \
  --service-cidr=10.1.0.0/16 \
  --pod-network-cidr=10.244.0.0/16
  --apiserver-bind-port=64438

```
#### 重置
```shell
kubeadm reset
```

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config