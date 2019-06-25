---
title: kubernetes
date: 2019-03-27 22:12:04
tags:
---
```

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
setenforce 0
swapoff -a
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
# 安装
yum install -y kubectl kubelet kubeadm
# 开机启动
systemctl enable kubelet
# 启动
systemctl start kubelet



kubeadm init --kubernetes-version=1.15.0 --apiserver-advertise-address=10.0.0.101 --image-repository registry.aliyuncs.com/google_containers --service-cidr=10.1.0.0/16 --pod-network-cidr=10.244.0.0/16 -v=10

https://www.jianshu.com/p/b58c85436f0a

kubeadm init --kubernetes-version=1.15.0 --apiserver-advertise-address=10.0.0.106 --image-repository registry.aliyuncs.com/google_containers --service-cidr=10.1.0.0/16 --pod-network-cidr=10.244.0.0/16 -v=10

install cni
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml


```
```
pod 优先级调度
参考：
https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/
```
PriorityClass

crd
https://sq.163yun.com/blog/article/174980128954048512


```
Kubernetes 1.8开始要求关闭系统的Swap，如果不关闭，默认配置下kubelet将无法启动。可以通过kubelet的启动参数--fail-swap-on=false更改这个限制。

全局关闭：
关闭系统的Swap方法如下:

swapoff -a
  
修改 /etc/fstab 文件，注释掉 SWAP 的自动挂载，使用free -m确认swap已经关闭。 > swappiness参数调整，修改/etc/sysctl.d/k8s.conf添加下面一行：

vm.swappiness=0
  
执行sysctl -p /etc/sysctl.d/k8s.conf使修改生效。
```



etcdctl --endpoints=https://[127.0.0.1]:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt --key=/etc/kubernetes/pki/etcd/healthcheck-client.key get foo



export HOST0=10.0.0.106
export HOST1=10.0.0.101
export HOST2=10.0.0.108

mkdir -p /tmp/${HOST0}/ /tmp/${HOST1}/ /tmp/${HOST2}/

ETCDHOSTS=(${HOST0} ${HOST1} ${HOST2})
NAMES=("infra0" "infra1" "infra2")

for i in "${!ETCDHOSTS[@]}"; do
HOST=${ETCDHOSTS[$i]}
NAME=${NAMES[$i]}
cat << EOF > /tmp/${HOST}/kubeadmcfg.yaml
apiVersion: "kubeadm.k8s.io/v1beta2"
kind: ClusterConfiguration
etcd:
    local:
        serverCertSANs:
        - "${HOST}"
        peerCertSANs:
        - "${HOST}"
        extraArgs:
            initial-cluster: ${NAMES[0]}=https://${ETCDHOSTS[0]}:2380,${NAMES[1]}=https://${ETCDHOSTS[1]}:2380,${NAMES[2]}=https://${ETCDHOSTS[2]}:2380
            initial-cluster-state: new
            name: ${NAME}
            listen-peer-urls: https://${HOST}:2380
            listen-client-urls: https://${HOST}:2379
            advertise-client-urls: https://${HOST}:2379
            initial-advertise-peer-urls: https://${HOST}:2380
EOF
done