# K8s集群部署node-exporter+prometheus+grafana监控系统

### 环境说明：
* k8s集群
* kubernetes version: v1.22.1
* docker version:20.10.17, build 100c701

>> 注：有些镜像需要根据k8s版本进行配置，下面的配置仅在上面描述环境有效，仅供参考。


* 创建k8s的命名空间为`monitoring`方便管理。
```powershell
kubectl create ns monitoring
```

* 在用户目录创建`k8s-monitoring`目录，再保存以下配置文件：

## 配置文件如下

### 1. 数据源pod

* `vim node-exporter.yaml`

```yml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-exporter
  namespace: monitoring
  labels:
    k8s-app: node-exporter
spec:
  selector:
    matchLabels:
        k8s-app: node-exporter
  template:
    metadata:
      labels:
        k8s-app: node-exporter
    spec:
      tolerations:
        - effect: NoSchedule
          key: node-role.kubernetes.io/master
      containers:
      - image: prom/node-exporter:v1.5.0 
        imagePullPolicy: IfNotPresent
        name: prometheus-node-exporter
        ports:
        - containerPort: 9100
          hostPort: 9100
          protocol: TCP
          name: metrics
        volumeMounts:
        - mountPath: /host/proc
          name: proc
        - mountPath: /host/sys
          name: sys
        - mountPath: /host
          name: rootfs
        args:
        - --path.procfs=/host/proc
        - --path.sysfs=/host/sys
        - --path.rootfs=/host
      volumes:
        - name: proc
          hostPath:
            path: /proc
        - name: sys
          hostPath:
            path: /sys
        - name: rootfs
          hostPath:
            path: /
      hostNetwork: true
      hostPID: true
---
apiVersion: v1
kind: Service
metadata:
  annotations:
    prometheus.io/scrape: "true"
  labels:
    k8s-app: node-exporter
  name: node-exporter
  namespace: monitoring
spec:
  type: NodePort
  ports:
  - name: http
    port: 9100
    nodePort: 30001
    protocol: TCP
  selector:
    k8s-app: node-exporter
```

部署`Node Exporter`

```powershell
$ kubectl apply -f node-exporter.yaml -n monitoring
daemonset.apps/node-exporter created
service/node-exporter created
$ kubectl get pod -n monitoring
NAME                  READY   STATUS    RESTARTS   AGE
node-exporter-9hmxv   1/1     Running   0          88s
node-exporter-m9kks   1/1     Running   0          88s
node-exporter-vzhtt   1/1     Running   0          88s
```

 验证`node_exporter`数据,打开浏览器输入`http://43.192.26.228:30001/metrics`:

 ![](https://s2.loli.net/2023/02/24/5phxX3sdlryJI42.png)

#### process exporter 进程采集器
* process_exporter进程采集器，node_exporter采集器不能对机器全部信息进行收集，所以对单个进程监控还需部署一个进程采集器，下面是配置文件参数说明：
* process exporter的配置文件为yaml格式，需要在配置文件中指明需要监控的进程，并将进程分组，一个进程只能属于一个组。
* 基本格式：
```yml
process_names:
  - name: "{{.Matches}}" # 匹配模板
    cmdline: 
    - 'redis-server' # 匹配名称
```

* 匹配模板
| {{.Comm}}      | 包含原始可执行文件的名称，即/proc/<pid>/stat       |
| -------------- | -------------------------------------------------- |
| {{.ExeBase}}   | 包含可执行文件的名称(默认)                         |
| {{.ExeFull}}   | 包含可执行文件的路径                               |
| {{.Username}}  | 包含的用户名                                       |
| {{.Matches}}   | 包含所有正则表达式而产生的匹配项（建议使用）       |
| {{.PID}}       | 包含进程的PID，一个PID仅包含一个进程（不建议使用） |
| {{.StartTime}} | 包含进程的开始时间（不建议使用）                   |

* process exporter常用指标
| namedprocess_namegroup_num_procs                | 运行的进程数                                      |
| ----------------------------------------------- | ------------------------------------------------- |
| namedprocess_namegroup_states                   | Running/Sleeping/Other/Zombie状态的进程数         |
| namedprocess_namegroup_cpu_seconds_total        | 获取/proc/[pid]/stat 进程CPU utime、stime状态时间 |
| namedprocess_namegroup_read_bytes_total         | 获取/proc/[pid]/io 进程读取字节数                 |
| namedprocess_namegroup_write_bytes_total        | 获取/proc/[pid]/io 进程写入字节数                 |
| namedprocess_namegroup_memory_bytes             | 获取进程使用的内存字节数                          |
| namedprocess_namegroup_open_filedesc            | 获取进程使用的文件描述符数量                      |
| namedprocess_namegroup_thread_count             | 运行的线程数                                      |
| namedprocess_namegroup_thread_cpu_seconds_total | 获取线程CPU状态时间                               |
| namedprocess_namegroup_thread_io_bytes_total    | 获取线程IO字节数                                  |

下面是进程监控的配置文件：`process-exporter.yaml`
```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: process-exporter-config
  namespace: monitoring
data:
  process-exporter-config.yaml: |-
    process_names:
    - name: "{{.Matches}}"
      cmdline:
      - 'docker'
    - name: "{{.Matches}}"
      cmdline:
      - 'kubelet'
    - name: "{{.Matches}}"
      cmdline:
      - 'chronyd'
    - name: "{{.Matches}}"
      cmdline:
      - 'sshd'
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: process-exporter
  namespace: monitoring
  labels:
    app: process-exporter
spec:
  selector:
    matchLabels:
      app: process-exporter
  template:
    metadata:
      labels:
        app: process-exporter
    spec:
      hostPID: true
      hostIPC: true
      hostNetwork: true
      nodeSelector:
        kubernetes.io/os: linux
      containers:
        - name: process-exporter
          image: ncabatoff/process-exporter
          args:
            - -config.path=/config/process-exporter-config.yaml
          ports:
            - containerPort: 9256
          resources:
            requests:
              cpu: 10m
              memory: 10Mi
            limits:
              cpu: 150m
              memory: 180Mi
          securityContext:
            runAsNonRoot: true
            runAsUser: 65534
          volumeMounts:
            - name: proc
              mountPath: /proc
            - name: config
              mountPath: /config
      volumes:
        - name: proc
          hostPath:
            path: /proc
        - name: config
          configMap:
            name: process-exporter-config
```
### 2.Prometheus相关

* `vim prometheus-configmap.yaml`

```yml
kind: ConfigMap
apiVersion: v1
metadata:
  labels:
    app: prometheus
  name: prometheus-config
  namespace: monitoring 
data:
  prometheus.yml: |
    #	Prometheus配置文件内容
    global:
      scrape_interval: 15s    #采集目标间隔时间
      scrape_timeout: 10s     #采集超时时间
      evaluation_interval: 1m #触发告警时间
    scrape_configs:           #配置数据源，称为 target，每个 target 用 job_name 命名。又分为静态配置和服务发现
    - job_name: 'kubernetes-node'
      kubernetes_sd_configs:
      - role: node
      relabel_configs:
      - source_labels: [__address__]
        regex: '(.*):10250'
        replacement: '${1}:9100'
        target_label: __address__
        action: replace
      - action: labelmap
        regex: __meta_kubernetes_node_label_(.+)
    - job_name: 'kubernetes-node-cadvisor'
      kubernetes_sd_configs:
      - role:  node
      scheme: https
      tls_config:
        ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
      relabel_configs:
      - action: labelmap
        regex: __meta_kubernetes_node_label_(.+)
      - target_label: __address__
        replacement: kubernetes.default.svc:443
      - source_labels: [__meta_kubernetes_node_name]
        regex: (.+)
        target_label: __metrics_path__
        replacement: /api/v1/nodes/${1}/proxy/metrics/cadvisor
    - job_name: 'kubernetes-apiserver'
      kubernetes_sd_configs:
      - role: endpoints
      scheme: https
      tls_config:
        ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
      relabel_configs:
      - source_labels: [__meta_kubernetes_namespace, __meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
        action: keep
        regex: default;kubernetes;https
    - job_name: 'kubernetes-service-endpoints'
      kubernetes_sd_configs:
      - role: endpoints
      relabel_configs:
    - job_name: 'process-exporter'
      scrape_interval: 1m
      scrape_timeout: 1m
      kubernetes_sd_configs:
      - role: node
      relabel_configs:
      - source_labels: [__address__]
        regex: '(.*):10250'
        replacement: '${1}:9256'
        target_label: __address__
        action: replace
      - action: labelmap
        regex: __meta_kubernetes_node_label_(.+)
      - source_labels: [__meta_kubernetes_node_address_InternalIP]
        action: replace
        target_label: ip

```

部署`prometheus-configmap.yaml`：

```powershell
$ kubectl apply -f prometheus-configmap.yaml -n monitoring
configmap/prometheus-config created
```

创建文件夹为Prometheus 数据的存放路径

```powershell
mkdir -p /data/prometheusdata
chmod 755 /data/prometheusdata
chown 65534.65534 /data/prometheusdata -R 
```

* `vim prometheus-deployment-service.yaml`

```yml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus-server
  namespace: monitoring
  labels:
    app: prometheus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
      component: server
    #matchExpressions:
    #- {key: app, operator: In, values: [prometheus]}
    #- {key: component, operator: In, values: [server]}
  template:
    metadata:
      labels:
        app: prometheus
        component: server
      annotations:
        prometheus.io/scrape: 'false'
    spec:
      nodeName: master
      serviceAccountName: monitoring
      containers:
      - name: prometheus
        image: prom/prometheus
        imagePullPolicy: IfNotPresent
        command:
          - prometheus
          - --config.file=/etc/prometheus/prometheus.yml 
          - --storage.tsdb.path=/prometheus #旧数据存储目录
          - --storage.tsdb.retention=720h   #何时删除旧数据，默认为 15 天。
          - --web.enable-lifecycle          #开启热加载
        ports:
        - containerPort: 9090
          protocol: TCP
        volumeMounts:
        - mountPath: /etc/prometheus/prometheus.yml
          name: prometheus-config
          subPath: prometheus.yml
        - mountPath: /prometheus/
          name: prometheus-storage-volume
      volumes:
        - name: prometheus-config
          configMap:
            name: prometheus-config
            items:
              - key: prometheus.yml
                path: prometheus.yml
                mode: 0644
        - name: prometheus-storage-volume
          hostPath:
           path: /data/prometheusdata
           type: Directory
---
apiVersion: v1
kind: Service
metadata:
  name: prometheus
  namespace: monitoring
  labels:
    app: prometheus
spec:
  type: NodePort
  ports:
    - port: 9090
      targetPort: 9090
      nodePort: 30002
      protocol: TCP
  selector:
    app: prometheus
    component: server
# 想要使配置生效可用如下命令热加载： 
# curl -X POST http://podIP:9090/-/reload
```

部署`prometheus-deployment.yaml`：

```powershell
$ kubectl create serviceaccount monitoring -n monitoring
$ kubectl create clusterrolebinding monitor-clusterrolebinding -n monitoring --clusterrole=cluster-admin --serviceaccount=monitoring:monitoring
$ kubectl apply -f prometheus-deployment-service.yaml -n monitoring
```
打开浏览器输入`http://43.192.26.228:30002/`,即可查看prometheus监控的tagets和一些node exporter采集到的数据：

![](https://s2.loli.net/2023/02/24/4WOrJNnqKtke96w.png)

![](https://s2.loli.net/2023/02/24/dHm4PiI6Jwn2BoX.png)

配置告警规则

* `vim prometheus-rules.yaml`

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-rules
  namespace: monitoring
data:
  kube-state-metrics.rules.yml: |
    groups:
    - name: host_status_alter
      rules:
      - alert: hostCpuUsageAlert
        expr: ceil(sum (avg without (cpu)(irate(node_cpu_seconds_total{mode!='idle'}[5m]))) by (instance)*100) > 90
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: CpuUsage alert
          description: "节点：{{ $labels.instance }} CPU 使用率大于 90% (当前值： {{ $value }}%)"
      - alert: hostMemUsageAlert
        expr: ceil((node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes)/node_memory_MemTotal_bytes*100 ) > 90
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: MemUsage alter
          description: "节点：{{ $labels.instance }} 内存使用率超过90% (当前值: {{ $value }}%)"
      - alert: OutOfDiskSpace-data
        expr: ceil(((1 - (node_filesystem_avail_bytes{fstype="xfs",mountpoint="/etc/resolv.conf"}) / node_filesystem_size_bytes{fstype="xfs",mountpoint="/etc/resolv.conf"}) * 100)) > 85
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "DiskSpace alter"
          description: "数据盘: /Data 使用率超过85% (当前值: {{ $value }})"
      - alert: network receive alert
        expr: ceil(sum by (instance) (irate(node_network_receive_bytes_total[2m])) / 1024 / 1024 )  > 50
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "network receive too much data alert"
          description: "{{ $labels.instance }} 网络接收流量超出负载 (> 50 MB/s) (当前值: {{ $value }})"
      - alert: network send too much data alert
        expr: ceil(sum by (instance) (irate(node_network_transmit_bytes_total[2m])) / 1024 / 1024) > 50
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "network send too much data alter"
          description: "{{ $labels.instance }}网络发送流量超出负载 (> 50 MB/s) (当前值: {{ $value }})"
      - alert: UnusualDiskReadRate
        expr: ceil(sum by (instance) (irate(node_disk_read_bytes_total[2m])) /1024 /1024) > 50
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Unusual disk read rate (instance {{ $labels.instance }})"
          description: "Disk is probably reading too much data (> 50 MB/s) (current value: {{ $value }})"
      - alert: UnusualDiskWriteRate
        expr: ceil(sum by (instance) (irate(node_disk_written_bytes_total[2m])) / 1024 / 1024) > 50
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Unusual disk write rate (instance {{ $labels.instance }})"
          description: "Disk is probably writing too much data (> 50 MB/s) (current value: {{ $value }})"
                  
      - alert: SslCertificateWillExpireSoon
        expr: probe_ssl_earliest_cert_expiry - time() < 86400 * 30
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "SSL certificate will expire soon (instance {{ $labels.instance }})"
          description: "SSL certificate expires in 30 days (current value: {{ $value }})"
      - alert: SslCertificateHasExpired
        expr: probe_ssl_earliest_cert_expiry - time()  <= 0
        for: 5m
        labels:
          severity: error
        annotations:
          summary: "SSL certificate has expired (instance {{ $labels.instance }})"
          description: "SSL certificate has expired already (current value: {{ $value }})"
      - alert: BlackboxSlowPing
        expr: probe_icmp_duration_seconds > 2
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Blackbox slow ping (instance {{ $labels.instance }})"
          description: "Blackbox ping took more than 2s (current value: {{ $value }})"
      - alert: BlackboxSlowRequests
        expr: probe_http_duration_seconds > 2 
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Blackbox slow requests (instance {{ $labels.instance }})"
          description: "Blackbox request took more than 2s (current value: {{ $value }})"
      - alert: PodCpuUsagePercent
        expr: sum(sum(label_replace(irate(container_cpu_usage_seconds_total[1m]),"pod","$1","container_label_io_kubernetes_pod_name", "(.*)"))by(pod) / on(pod) group_right kube_pod_container_resource_limits_cpu_cores *100 )by(container,namespace,node,pod,severity) > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Pod cpu usage percent has exceeded 80% (current value: {{ $value }}%)"
    - name: cluster_status_alter
      rules:
      - alert: DeploymentGenerationMismatch
        expr: kube_deployment_status_observed_generation != kube_deployment_metadata_generation
        for: 5m
        labels:
          severity: warning
        annotations:
          description: |
            Observed deployment generation does not match expected one for
            deployment {{$labels.namespace}}/{{$labels.deployment}}
          summary: Deployment is outdated
      - alert: DeploymentReplicasNotUpdated
        expr: |
          ((kube_deployment_status_replicas_updated != kube_deployment_spec_replicas)
          or (kube_deployment_status_replicas_available != kube_deployment_spec_replicas))
          unless (kube_deployment_spec_paused == 1)
        for: 5m
        labels:
          severity: warning
        annotations:
          description: Replicas are not updated and available for deployment {{$labels.namespace}}/{{$labels.deployment}}
          summary: Deployment replicas are outdated
      - alert: DaemonSetRolloutStuck
        expr: kube_daemonset_status_number_ready / kube_daemonset_status_desired_number_scheduled * 100 < 100
        for: 5m
        labels:
          severity: warning
        annotations:
          description: |
            Only {{$value}}% of desired pods scheduled and ready for daemon
            set {{$labels.namespace}}/{{$labels.daemonset}}
          summary: DaemonSet is missing pods
      - alert: K8SDaemonSetsNotScheduled
        expr: kube_daemonset_status_desired_number_scheduled - kube_daemonset_status_current_number_scheduled  > 0
        for: 10m
        labels:
          severity: warning
        annotations:
          description: A number of daemonsets are not scheduled.
          summary: Daemonsets are not scheduled correctly
      - alert: DaemonSetsMissScheduled
        expr: kube_daemonset_status_number_misscheduled > 0
        for: 10m
        labels:
          severity: warning
        annotations:
          description: A number of daemonsets are running where they are not supposed to run.
          summary: Daemonsets are not scheduled correctly
      - alert: PodFrequentlyRestarting
        expr: ceil(increase(kube_pod_container_status_restarts_total[30m])) > 3
        for: 5m
        labels:
          severity: warning
        annotations:
          description: "Pod: {{$labels.namespace}}/{{$labels.pod}} 30分钟内重启次数超过3次，当前值:{{ $value }}"
          summary: Pod is restarting frequently
      - alert: K8SNodeStatusFailure
        expr: kube_node_status_condition{condition="Ready",status!="true"}==1
        labels:
          severity: hight
        annotations:
          description: Node {{$labels.namespace}}/{{$labels.node}} status failure.
          summary: 集群节点状态错误
      - alert: ContainerMemoryAlter
        expr: ceil((sum(container_memory_working_set_bytes{pod_name=~".*",container_name!=""}/1024/1024 != 0)by (namespace,container_name,instance) / sum(container_spec_memory_limit_bytes{pod_name=~".*",container_name!=""}/1024/1024 != 0)by (namespace,container_name,instance))*100 ) > 95
        labels:
          severity: hight
        annotations:
          description: "名称空间:{{ $labels.namespace }} 容器{{ $labels.container_name }}内存使用量超过内存限制总量(limit)的95%当前值:{{ $value }}%"
          summary: 容器内存使用量过高
      - alert: ContainerCpuAlter
        for: 1m
        expr: ceil((sum(irate(container_cpu_usage_seconds_total{pod_name=~".*",container_name!="",container_name!="POD"}[30s])) by (namespace,container_name,instance) != 0  / sum(container_spec_cpu_quota{pod_name=~".*",container_name!="",container_name!="POD"} / container_spec_cpu_period{pod_name=~".*",container_name!="",container_name!="POD"}) by (namespace,container_name,instance) ) * 100 ) > 95
        labels:
          severity: hight
        annotations:
          description: "名称空间:{{ $labels.namespace }} 容器{{ $labels.container_name }} cpu使用率超过限制总量(limit)的95%当前值:{{ $value }}%"
          summary: 容器cpu使用量过高
        # api接口、服务监控
      - alert: HttpStatusCodeAlter
        expr: probe_http_status_code{job="web_status"} <= 199 OR probe_http_status_code >= 400
        for: 1m
        labels:
          severity: error
        annotations:
          summary: "Status Code (接口：{{ $labels.domain }})"
          description: "HTTP status code is not 200-399 (current value: {{ $value }})"
      - alert: ServicePortAlter
        expr: probe_success{job="tcp-consul-blackbox"} == 0
        for: 1m
        labels:
          severity: hight
        annotations:
          summary: "{{ $labels.app }} server is unreachable"
          description: "{{ $labels.app }}：{{ $labels.service }}挂了,请及时处理"
```

部署`prometheus-rules.yaml.yaml`

```powershell
$ kubectl apply -f prometheus-rules.yaml -n monitoring
```
最后还需要配置一下prometheus的RBAC。

* `vim prometheus-rbac.yaml`
```yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: prometheus
  namespace: monitoring

---
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: monitor-token
  namespace: monitoring
  annotations:
    kubernetes.io/service-account.name: "prometheus"
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: prometheus
rules:
- apiGroups:
  - ""
  resources:
  - nodes
  - services
  - endpoints
  - pods
  - nodes/proxy
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - "extensions"
  resources:
    - ingresses
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  resources:
  - configmaps
  - nodes/metrics
  verbs:
  - get
- nonResourceURLs:
  - /metrics
  verbs:
  - get
---
#apiVersion: rbac.authorization.k8s.io/v1beta1
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: prometheus
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: prometheus
subjects:
- kind: ServiceAccount
  name: prometheus
  namespace: monitoring
```
部署`prometheus-rbac.yaml`：
```powershell
$ kubectl apply -f prometheus-rbac.yaml -n monitoring
serviceaccount/prometheus created
secret/monitor-token created
clusterrole.rbac.authorization.k8s.io/prometheus created
clusterrolebinding.rbac.authorization.k8s.io/prometheus created
```
### 3. 安装kube-state-metrics，注意与k8s对应的版本，不然会运行不起来，Github地址：[kube-state-metrics](https://github.com/kubernetes/kube-state-metrics)
* `vim kube-state-metrics-deploy.yaml`
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kube-state-metrics
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kube-state-metrics
  template:
    metadata:
      labels:
        app: kube-state-metrics
    spec:
      nodeName: master
      serviceAccountName: kube-state-metrics
      containers:
      - name: kube-state-metrics
        image: registry.cn-hangzhou.aliyuncs.com/zhangshijie/kube-state-metrics:v2.3.0
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: kube-state-metrics
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: kube-state-metrics
rules:
- apiGroups: [""]
  resources: ["nodes", "pods", "services", "resourcequotas", "replicationcontrollers", "limitranges", "persistentvolumeclaims", "persistentvolumes", "namespaces", "endpoints"]
  verbs: ["list", "watch"]
- apiGroups: ["extensions"]
  resources: ["daemonsets", "deployments", "replicasets"]
  verbs: ["list", "watch"]
- apiGroups: ["apps"]
  resources: ["statefulsets"]
  verbs: ["list", "watch"]
- apiGroups: ["batch"]
  resources: ["cronjobs", "jobs"]
  verbs: ["list", "watch"]
- apiGroups: ["autoscaling"]
  resources: ["horizontalpodautoscalers"]
  verbs: ["list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: kube-state-metrics
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: kube-state-metrics
subjects:
- kind: ServiceAccount
  name: kube-state-metrics
  namespace: kube-system

---
apiVersion: v1
kind: Service
metadata:
  annotations:
    prometheus.io/scrape: 'true'
  name: kube-state-metrics
  namespace: kube-system
  labels:
    app: kube-state-metrics
spec:
  type: NodePort
  ports:
  - name: kube-state-metrics
    port: 8080
    targetPort: 8080
    nodePort: 30003
    protocol: TCP
  selector:
    app: kube-state-metrics
```
部署`kube-state-metrics-deploy.yaml`
```powershell
$ kubectl apply -f kube-state-metrics-deploy.yaml 
deployment.apps/kube-state-metrics created
serviceaccount/kube-state-metrics created
clusterrole.rbac.authorization.k8s.io/kube-state-metrics created
clusterrolebinding.rbac.authorization.k8s.io/kube-state-metrics created
service/kube-state-metrics created
$ kubectl get pod -A  | grep kube-state-metrics
kube-system   kube-state-metrics-6946f8dc55-767pg   0/1     ImagePullBackOff   0          29s
$ kubectl get svc -n kube-system
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
kube-dns             ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   142m
kube-state-metrics   NodePort    10.96.0.88   <none>        8080:30003/TCP           56s
```
打开浏览器输入`http://43.192.26.228:30003/`可以看到Kube Metrics：

![](https://s2.loli.net/2023/02/24/ATnByLZV2m6urGK.png)

![](https://s2.loli.net/2023/02/24/P5WAHsu69wNYcRM.png)

![](https://s2.loli.net/2023/02/24/FxQqKyHW7P6Rsjb.png)

### 4. grafana 部署

* `vim grafana.yaml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-enterprise
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana-enterprise
  template:
    metadata:
      labels:
        app: grafana-enterprise
    spec:
      nodeName: master
      containers:
      - image: grafana/grafana
        imagePullPolicy: Always
        #command:
        #  - "tail"
        #  - "-f"
        #  - "/dev/null"
        securityContext:
          allowPrivilegeEscalation: false
          runAsUser: 0
        name: grafana
        ports:
        - containerPort: 3000
          protocol: TCP
        volumeMounts:
        - mountPath: "/var/lib/grafana"
          name: data
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
          limits:
            cpu: 500m
            memory: 2500Mi
      volumes:
      - name: data
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: grafana
  namespace: monitoring
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 3000
    nodePort: 30000
  selector:
    app: grafana-enterprise
```
部署`grafana.yaml`：
```powershell
$ kubectl apply -f grafana.yaml -n monitoring
deployment.apps/grafana-enterprise created
service/grafana created
$ kubectl get svc -n monitoring
NAME            TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
grafana         NodePort   10.109.233.102   <none>        80:30000/TCP     18s
node-exporter   NodePort   10.100.169.96    <none>        9100:30001/TCP   84m
prometheus      NodePort   10.109.131.138   <none>        9090:30002/TCP   51m

```
打开浏览器输入`http://43.192.26.228:30000/login`访问Grafana:

![](https://s2.loli.net/2023/02/24/CMNn2lahFZzjfQp.png)

输入用户名密码：
* user：admin
* psw:admin
* 更改后的密码为：Lemu@1215

登录成功后，开始添加数据源：

![](https://s2.loli.net/2023/02/24/Fn85fp9zEiNTJwt.png)

选择Prometheus

![](https://s2.loli.net/2023/02/24/nJibuwjyKQ62Agm.png)

配置如下图：
* 数据源地址：`http://43.192.26.228:30002/`

![](https://s2.loli.net/2023/02/24/xofAWplVycIg21U.png)

导入数据面板并展示：选择最左边的`+`号再选择`Import`,进到以下界面：按照以下配置，输入`3119`点击`Load`

![](https://s2.loli.net/2023/02/27/nCIibxXZJrBNgwG.png)

面板名字默认（可修改），数据源选择`Prometheus`,点击`Import`

![](https://s2.loli.net/2023/02/27/GIntxT4CaDSYyk2.png)

成功后可以看到下面的面板：

![](https://s2.loli.net/2023/02/27/LtQOZl1I7qwN9eo.png)

可以切换不同节点的监控状态：

![](https://s2.loli.net/2023/02/27/Y3RPmDgNkFBtdqS.png)


* **常用的Grafana DashBoard 仪表盘推荐：**
* [Node Exporter Dashboard 22/04/13 ConsulManager自动同步版](https://grafana.com/grafana/dashboards/8919-1-node-exporter-for-prometheus-dashboard-cn-0413-consulmanager/):ID: `8919`
* [Kubernetes cluster monitoring (via Prometheus)](https://grafana.com/grafana/dashboards/3119-kubernetes-cluster-monitoring-via-prometheus/):ID:`3119`
