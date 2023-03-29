# 升级 `kubeadm` 集群
由于上面部署集群使用的`kubeadm`的版本太低了，需要对`kubeadm`创建的 Kubernetes 集群从 `1.17.3` 版本 升级到 `1.18.20` 版本，逐步更新到最新的`1.26.1`版本。

升级工作的基本流程如下：
1. 升级主控制平面节点
2. 升级其他控制平面节点
3. 升级工作节点

### 1. 准备工作
开始升级之前我们先做一些准备工作：
1. 仔细阅读[版本更新说明](https://github.com/kubernetes/kubernetes/tree/master/CHANGELOG)：主要关注当前版本到目标升级版本之间的变更，尤其是大版本升级更加要注意；
2. K8S集群必须使用静态控制面节点
3. K8S集群的etcd必须是静态pod部署或者是外置etcd
4. **备份**重要的数据和业务：虽然kubeadm升级只涉及k8s的内部组件，但是对于一些重要业务和app级别的有状态服务，还是有备无患比较放心
5. 禁用集群的SWAP内存

一些注意事项：
1. 对于任何kubelet的小版本升级（minor version upgrade），**一定要先驱逐该node上面的所有负载（`drain the node`）**，避免残留一些诸如coredns之类的重要workload影响整个集群的稳定性；
2. 由于`container`的`spec hash value`在集群升级后会发生变更，因此所有的`container`**在集群升级完成后都会重启**；
3. 可以使用`systemctl status kubele`t或者`journalctl -xeu kubelet`来查看kubelet的日志从而确定其升级是否成功；
4. 不建议在`kubeadm upgrade`升级集群的同时使用`--config`来重新配置集群，如果需要更新集群的配置，可以参考这篇[官方教程](https://kubernetes.io/zh-cn/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)；

### 2. 升级`kubeadm`（在master和node节点运行）
在升级集群之前需要先对集群上面的所有节点的`kubeadm`都进行升级,由于每次升级只能跨一个版本升级，所以这里使用`yum`升级到`1.18.20`版本。
```powershell
# 查看所有可用的kubeadm的版本
yum list --showduplicates kubeadm --disableexcludes=kubernetes

# 然后升级kubeadm的版本到1.18.20
yum install -y kubeadm-1.18.20-0 --disableexcludes=kubernetes
```
完成之后我们检查`kubeadm`的版本信息，顺利输出下面的信息则说明升级成功
```powershell
$ kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.20", GitCommit:"1f3e19b7beb1cc0110255668c4238ed63dadb7ad", GitTreeState:"clean", BuildDate:"2021-06-16T12:56:41Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
```

### 3. 升级控制平面节点
#### 3.1. 升级K8S组件(在master节点执行)
```powershell
# 查看升级计划
$ kubeadm upgrade plan
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[preflight] Running pre-flight checks.
[upgrade] Running cluster health checks
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: v1.17.3
[upgrade/versions] kubeadm version: v1.18.20
I0223 12:05:15.217642   13143 version.go:255] remote version is much newer: v1.26.1; falling back to: stable-1.18
[upgrade/versions] Latest stable version: v1.18.20
[upgrade/versions] Latest stable version: v1.18.20
[upgrade/versions] Latest version in the v1.17 series: v1.17.17
[upgrade/versions] Latest version in the v1.17 series: v1.17.17

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT       AVAILABLE
Kubelet     3 x v1.17.3   v1.17.17

Upgrade to the latest version in the v1.17 series:

COMPONENT            CURRENT   AVAILABLE
API Server           v1.17.3   v1.17.17
Controller Manager   v1.17.3   v1.17.17
Scheduler            v1.17.3   v1.17.17
Kube Proxy           v1.17.3   v1.17.17
CoreDNS              1.6.5     1.6.7
Etcd                 3.4.3     3.4.3-0

You can now apply the upgrade by executing the following command:

	kubeadm upgrade apply v1.17.17

_____________________________________________________________________

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT       AVAILABLE
Kubelet     3 x v1.17.3   v1.18.20

Upgrade to the latest stable version:

COMPONENT            CURRENT   AVAILABLE
API Server           v1.17.3   v1.18.20
Controller Manager   v1.17.3   v1.18.20
Scheduler            v1.17.3   v1.18.20
Kube Proxy           v1.17.3   v1.18.20
CoreDNS              1.6.5     1.6.7
Etcd                 3.4.3     3.4.3-0

You can now apply the upgrade by executing the following command:

	kubeadm upgrade apply v1.18.20

_____________________________________________________________________
# 执行提示命令更新kubeadm为1.18.20，提示输入“y”，即可进行升级
kubeadm upgrade apply v1.18.20
```
看到最后输出类似的信息就说明该节点已经升级成功。
```powershell
[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.18.20". Enjoy!

[upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.
```
这时只是`master`节点机器升级了`kubeadm`的版本，可以再使用`kubeadm upgrade plan`查看升级计划：
```powershell
$ kubeadm upgrade plan
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[preflight] Running pre-flight checks.
[upgrade] Running cluster health checks
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: v1.18.20
[upgrade/versions] kubeadm version: v1.18.20
I0223 12:14:57.089759   21062 version.go:255] remote version is much newer: v1.26.1; falling back to: stable-1.18
[upgrade/versions] Latest stable version: v1.18.20
[upgrade/versions] Latest stable version: v1.18.20
[upgrade/versions] Latest version in the v1.18 series: v1.18.20
[upgrade/versions] Latest version in the v1.18 series: v1.18.20

Awesome, you're up-to-date! Enjoy!
```
对`node`节点机器执行以下更新命令：
```powershell
kubeadm upgrade node
```
当看到类似的输出信息时则说明升级成功
```powershell
[upgrade] Reading configuration from the cluster...
[upgrade] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[upgrade] Upgrading your Static Pod-hosted control plane instance to version "v1.18.20"...
Static pod: kube-apiserver-master hash: 284f29e1d8d7199a418de32c45dff8f7
Static pod: kube-controller-manager-master hash: 0ebd28c38124a07df5ed7eebbb63aa07
Static pod: kube-scheduler-master hash: 1405584ea6063e7eb370d80efb6b4b64
[upgrade/etcd] Upgrading to TLS for etcd
[upgrade/etcd] Non fatal issue encountered during upgrade: the desired etcd version for this Kubernetes version "v1.18.20" is "3.4.3-0", but the current etcd version is "3.4.3". Won't downgrade etcd, instead just continue
[upgrade/staticpods] Writing new Static Pod manifests to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests815652469"
W0223 12:19:13.657144   23687 manifests.go:225] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[upgrade/staticpods] Preparing for "kube-apiserver" upgrade
[upgrade/staticpods] Current and new manifests of kube-apiserver are equal, skipping upgrade
[upgrade/staticpods] Preparing for "kube-controller-manager" upgrade
[upgrade/staticpods] Current and new manifests of kube-controller-manager are equal, skipping upgrade
[upgrade/staticpods] Preparing for "kube-scheduler" upgrade
[upgrade/staticpods] Current and new manifests of kube-scheduler are equal, skipping upgrade
[upgrade] The control plane instance for this node was successfully updated!
[kubelet-start] Downloading configuration for the kubelet from the "kubelet-config-1.18" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[upgrade] The configuration for this node was successfully updated!
[upgrade] Now you should go ahead and upgrade the kubelet package using your package manager.
```
#### 3.2. 升级kubelet和kubectl
上面的更新操作完成之后我只是把K8S集群中的相关pod都升级了一遍，但是`kubelet`并没有升级，因此这里看到的版本信息还是`v1.17.3`
```powershell
$ kubectl get nodes
NAME     STATUS   ROLES    AGE   VERSION
master   Ready    master   27h   v1.17.3
node1    Ready    <none>   27h   v1.17.3
node2    Ready    <none>   27h   v1.17.3
```
升级`kubelet`之前我们要对节点进行驱逐操作，这里先将`master`的全部工作负载全部驱逐掉。
```powershell
$ kubectl drain master --ignore-daemonsets
node/master cordoned
evicting pod "coredns-7ff77c879f-6ptvv"
evicting pod "coredns-7ff77c879f-stcj9"
pod/coredns-7ff77c879f-6ptvv evicted
pod/coredns-7ff77c879f-stcj9 evicted
node/master evicted
```
接着就可以使用`yum`更新`kubelet`和`kubectl`到对应的`v1.18.20`版本：
```powershell
yum install -y kubelet-1.18.20-0 kubectl-1.18.20-0 --disableexcludes=kubernetes
systemctl daemon-reload
systemctl restart kubelet

# 查看日志检查相关服务是否正常
systemctl status kubelet -l
journalctl -xeu kubelet
```
这时候再查看`master`的状态就能看到节点已经升级成功，版本信息更新为`v1.18.20`
```powershell
$ kubectl get nodes
NAME     STATUS                     ROLES    AGE   VERSION
master   Ready,SchedulingDisabled   master   27h   v1.18.20
node1    Ready                      <none>   27h   v1.17.3
node2    Ready                      <none>   27h   v1.17.3
```
确定该节点正常后，就能恢复它的调度
```powershell
kubectl uncordon master
```
接着对剩下的两个节点进行同样的操作:
* node1
```powershell
kubectl drain node1 --ignore-daemonsets
yum install -y kubelet-1.18.20-0 kubectl-1.18.20-0 --disableexcludes=kubernetes
systemctl daemon-reload
systemctl restart kubelet

# 恢复node1的调度
kubectl uncordon node1
```
* node2
```powershell
kubectl drain node2 --ignore-daemonsets
yum install -y kubelet-1.18.20-0 kubectl-1.18.20-0 --disableexcludes=kubernetes
systemctl daemon-reload
systemctl restart kubelet

# 恢复node1的调度
kubectl uncordon node2
```  
都更新完成之后我们可以看到整个集群的控制面就完成升级了。
```powershell
$ kubectl get nodes -o wide
NAME     STATUS   ROLES    AGE   VERSION    INTERNAL-IP     EXTERNAL-IP   OS-IMAGE         KERNEL-VERSION                  CONTAINER-RUNTIME
master   Ready    master   27h   v1.18.20   43.192.26.228   <none>        Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   docker://20.10.17
node1    Ready    <none>   27h   v1.18.20   172.31.27.106   <none>        Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   docker://20.10.17
node2    Ready    <none>   27h   v1.18.20   52.83.66.169    <none>        Amazon Linux 2   5.10.165-143.735.amzn2.x86_64   docker://20.10.17
```
### 4. 升级失败
这里升级版本请一定一个版本一个版本升级，虽然步骤比较多，但是出现失败的概率是很低，如果垮了很大版本去升级的话，升级计划会提示版本差距太大导致无法升级，所以请按照官方的文档进行操作，万一真的出现失败的情况，官方给了一些升级失败的应付手段。
* **升级失败自动回滚**：这是不幸中的万幸，使用kubeadm升级集群节点如果失败了，理论上是会自动回滚到升级前的旧版本，如果回滚成功，那么此时的集群应该是可以正常工作的；
* **升级过程被意外中断**：升级过程中因为网络状况等各种问题被中断了，之后只需要再次执行之前的升级命令即可；因为官方声称kubeadm的操作是幂等的，也就是说只需要在升级完成后检查集群状态正常即可；
* **升级失败手动回滚**：`kubeadm`在升级之前会把相关的`manifests`文件和`etcd`的数据都备份到`/etc/kubernetes/tmp`目录下面，最坏的情况就是需要我们自己去手动恢复数据并重启相关服务。
* [从故障状态恢复](https://kubernetes.io/zh-cn/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#recovering-from-a-failure-state)


## `kubectl`命令自动补全功能
管理Kubernetes集群的时候，为了提高使用`kubectl`命令工具的便捷性，介绍一下`kubectl`命令补全工具的安装。
1. 安装`bash-completion`：
```powershell
yum install -y bash-completion 
source /usr/share/bash-completion/bash_completion
```
2. 应用`kubectl`的completion到系统环境：
```powershell
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```
3. 应用成功后可以使用`Tab`键来自动补全命令了。