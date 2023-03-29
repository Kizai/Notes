# 公网部署k8s集群的操作步骤

节点机器配置说明：

|名称|CPU|内存|操作系统|公网IP|
|--|---|--|--|--|
|master|2核|2G|AWS EC2|43.192.26.228|
|node1|2核|1G|AWS EC2|43.192.34.160|
|node2|2核|1G|AWS EC2|52.83.66.169|

>> 注意：master机器的防火墙需要开放`6443`端口。

### 1. 基础环境配置（所有节点请切换到`root`用户全部执行）

#### 1.1. 关闭防火墙(没有可以跳过)

```powershell
systemctl stop firewalld
```

#### 1.2. 关闭`selinux`

```powershell
sed -i 's/enforcing/disabled/' /etc/selinux/config
```

#### 1.3. 关闭`swap`

```powershell
echo "vm.swappiness = 0">> /etc/sysctl.conf 
swapoff -a && swapon -a
sysctl -p
# 可以使用free -h 来查看swap的大小
```

#### 1.4. 创建公网ip的虚拟网卡

由于我们使用的是公网ip，但是在云服务器中是没有对应的网卡的，这导致在 `kubeadm` 部署时使用公网ip有问题,所以要对所有公网ip的主机创建各自公网ip的虚拟网卡（该方法重启后会自动删除虚拟网卡需要重新创建，到时候使用脚本可以解决）

* 各节点安装以下软件

```powershell
# 安装软件包
modprobe tun
lsmod | grep tun

# 编辑文件
vim /etc/yum.repos.d/nux-misc.repo
# 填入下面的内容
[nux-misc]
name=Nux Misc
baseurl=http://li.nux.ro/download/nux/misc/el7/x86_64/
enabled=0
gpgcheck=1
gpgkey=http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro

# 安装
yum --enablerepo=nux-misc install tunctl
```

* `master`节点

```powershell
# 新建虚拟网卡
tunctl -t eth1
# 配置网卡的IP，注意替换ip成自己机器对应的公网IP
ifconfig eth1 43.192.26.228 netmask 255.255.255.0 up
# 查看配置
ifconfig
```

* `node1`节点

```powershell
# 新建虚拟网卡
tunctl -t eth1
# 配置网卡的IP，注意替换ip成自己机器对应的公网IP
ifconfig eth1 43.192.34.160 netmask 255.255.255.0 up
# 查看配置
ifconfig
```

* `node2`节点

```powershell
# 新建虚拟网卡
tunctl -t eth1
# 配置网卡的IP，注意替换ip成自己机器对应的公网IP
ifconfig eth1  52.83.66.169 netmask 255.255.255.0 up
# 查看配置
ifconfig
```

#### 1.6. 设置主机名称（配置完，使用`ssh`重新连接节点则可以看到更新后的主机名）

* `master`节点

```powershell
hostnamectl set-hostname master
```

* `node1`节点

```powershell
hostnamectl set-hostname node1
```

* `node2`节点

```powershell
hostnamectl set-hostname node2
```

#### 1.7. 修改hosts（注意：所有节点都需要添加各个节点的公网ip和主机名）

```powershell
# 编辑hosts文件
vim /etc/hosts
# 在配置hosts配置文件最下方添加公网ip映射
43.192.26.228 master
43.192.34.160 node1
52.83.66.169 node2
```

#### 1.8. 将桥接的 `IPv4` 流量传递到 `iptables` 的链

```powershell
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system # 刷新生效
```

#### 1.9. 安装docker（所有节点全部执行）

```powershell
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo
yum -y install docker-ce
systemctl enable docker && systemctl start docker
docker --version
```

#### 1.10. 安装`kubeadm`、`kubelet` 、`kubectl`（所有节点全部执行）

```powershell
# 添加阿里云 YUM 软件源 
cat > /etc/docker/daemon.json << EOF
{
"registry-mirrors": ["https://b9pmyelo.mirror.aliyuncs.com"]
} 
EOF
# 添加 Kubernetes yum 源
cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
# 安装 kubeadm、kubelet 和 kubectl，由于高版本部署不成功暂使用低版本进行部署
yum install -y kubelet-1.17.3 kubectl-1.17.3 kubeadm-1.17.3 
systemctl enable kubelet
```

#### 1.11. 修改启动参数(每个主机)

修改`systemd`管理的`kubectl`, 添加 kubelet的启动参数`--node-ip=公网IP`， 每个主机都要添加并指定对应的公网ip, 添加了这一步才能使用公网 ip来注册进集群:

```powershell
vim /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

在最后一行添加`--node-ip=<公网IP>`

# master 
--node-ip=43.192.26.228
# node1
--node-ip=43.192.34.160
# node2
--node-ip=52.83.66.169
```

![](https://s2.loli.net/2023/02/22/l3w8uiOoN2ZGhUx.png)

### 2. 启动`master`节点

#### 2.1 在`master`初级进行节点初始化

```powershell
# 执行以下命令使用kubeadm进行初始化
kubeadm init \
--apiserver-advertise-address=43.192.26.228  \
--image-repository registry.aliyuncs.com/google_containers \
--kubernetes-version v1.17.3 \
--service-cidr=10.96.0.0/12 \
--pod-network-cidr=10.244.0.0/16
```

参数解析：

* `apiserver-advertise-address` 是集群master节点的公网ip。
* `service-cidr` 是service网段。
* `pod-network-cidr` 是Pod网段。

初始化成功会有以下提示：

```powershell
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

# 生成字节点加入集群的命令，这个需要保存好
kubeadm join 43.192.26.228:6443 --token fm0hav.h0b2htc6nhms6ooh \
    --discovery-token-ca-cert-hash sha256:c1d40513d7d1c09b4b02646eb667248e54d4df727d47ea6155be76268bb28ff9
```

#### 2.2. 保存生成的配置文件(master节点运行)

```powershell
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```

#### 2.3. 在`node`节点运行`kubeadm join`加入主节点的时候经常会报`The connection to the server localhost:8080 was refused - did you specify the right host or port?`，这是`node`节点没有`conf`文件，所以需要复制`conf`文件到从节点，并设置环境变量就OK了

```powershell
# 在master节点复制admin.conf的内容到剪贴板
cat /etc/kubernetes/admin.conf
# node1和node2 节点运行
vim /etc/kubernetes/admin.conf
# 复制粘贴板的内容进去，保存推出，再执行以下命令即可
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```

#### 2.4. 安装pod网络插件

```powershell
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

### 3. 加入集群

#### 3.1. 加入k8s集群（node节点运行）

```powershell
# 复制master生成的kubeadm join...命令即可加入集群
kubeadm join 43.192.26.228:6443 --token fm0hav.h0b2htc6nhms6ooh \
    --discovery-token-ca-cert-hash sha256:c1d40513d7d1c09b4b02646eb667248e54d4df727d47ea6155be76268bb28ff9
```

#### 3.2. 重新获取`kubeadm join`命令

万一`kubeadm join`命令忘记了或者命令过期了，在`master`节点重新执行以下命令获取新的`kubeadm join`命令

```powershell
kubeadm token create --print-join-command
```

#### 3.3. 在各个节点查看节点的状态(每个节点都可以运行)

```powershell
[root@master ~]# kubectl get nodes
NAME     STATUS   ROLES    AGE   VERSION
master   Ready    master   70m   v1.17.3
node1    Ready    <none>   68m   v1.17.3
node2    Ready    <none>   67m   v1.17.3
```

### 4. 创建`pod`测试（在master节点执行）

#### 4.1. 查询现有的`pod`

```powershell
[root@master ~]# kubectl get pod,svc
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   71m
```

#### 4.2. 创建一个新的`nginx`的`pod`

```powershell
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get pod,svc

# 运行情况
[root@master ~]# kubectl get pod,svc
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-86c57db685-dj27c   1/1     Running   0          21m

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP        115m
service/nginx        NodePort    10.103.90.99   <none>        80:32276/TCP   5m43s
# 访问master的公网ip:32276应该可以看到nginx的界面，由于该端口没有在防火墙开放，所以暂时看不了。
```

#### 4.3. 删除`delpoyment`、`pod`、`service`
```powershell
# 删除单个delpoyment
kubectl delete deployment [deployment name]
# 删除全部
kubectl delete deployments --all

# 删除单个pod
kubectl delete pod [pod name]
# 删除全部pods
kubectl delete pods --all

# 删除service
kubectl delete svc [svc name]
```