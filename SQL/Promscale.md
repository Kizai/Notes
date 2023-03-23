# 使用 Promscale 存储 Prometheus 的指标数据到 psql

### [Promscale简介](https://github.com/timescale/promscale)

![](https://s2.loli.net/2023/03/21/1s3kzuvRaj9UW5D.png)

Promscale 是基于 PostgreSQL 和 TimescaleDB 构建的 Prometheus、Jaeger 和 OpenTelemetry 的统一指标和跟踪可观察性后端。

Promscale 可用作稳健且 100% 符合 PromQL 的 Prometheus 远程存储，以及持久且可扩展的 Jaeger 存储后端。Promscale 是经过认证的 Jaeger 存储后端。

与其他可观察性后端不同，它具有简单且易于管理的架构，只有两个组件：Promscale 连接器和 Promscale 数据库（带有 TimescaleDB 和 Promscale 扩展的 PostgreSQL）。

不过最近宣布对 Promscale 停止维护了，但毋庸置疑 Promscale 目前仍然是 Prometheus 远程存储最好的工具。

### 主要特征
* **Prometheus 指标存储**：支持远程写入、远程读取、100% PromQL、指标元数据、范例和 Prometheus HA。
* **经过认证的 Jaeger 跟踪存储**： Promscale 是[经过认证的 Jaeger 存储后端](https://github.com/jaegertracing/jaeger#multiple-storage-backends)。将 Jaeger 与 Promscale 集成，通过在 Jaeger 中进行简单的配置更改来存储和可视化您的踪迹。[使用 Promscale 作为服务性能管理](https://www.jaegertracing.io/docs/1.38/spm/) UI所需指标的存储后端 。无需单独的 Prometheus / PromQL 兼容存储。
* **OpenTelemetry 跟踪存储**：支持通过 OpenTelemetry 协议 (OTLP) 摄取跟踪。
* **Grafana 集成**：使用 PromQL、SQL 和 Jaeger 数据源查询和可视化您的指标和跟踪。
* **持久可靠的存储**：建立在成熟的 Postgres 和 TimescaleDB 之上，在全球拥有数百万个实例。提供高可用性、复制、数据完整性、数据压缩、备份、身份验证、角色和权限的可信系统。
* **PromQL 警报**：完全支持 PromQL 警报规则。您可以重用已有的 Prometheus 配置。
* **多租户**：支持 Prometheus 多租户，因此您可以限制租户的数据访问。
* **选择您的查询语言**：用于度量的 PromQL 和用于度量和跟踪的 SQL。借助完整的 SQL 支持以及 TimescaleDB 的高级分析功能，您可以查询和关联指标、跟踪和业务数据以获得新的见解。
* **灵活的数据管理**：可配置的指标和跟踪的默认保留以及每个指标的保留和用于删除不再需要的指标系列的 API。
* **下采样**：通过使用 PromQL 记录规则和 TimescaleDB 连续聚合对指标进行下采样来提高长期查询的性能。将缩减采样与按指标保留相结合，以仅保留您需要的数据，从而降低成本并提高性能。
* **开箱即用的监控**：利用 Promscale 团队构建的仪表板、警报规则和运行手册，从第一天开始监控 Promscale，遵循产品背后团队的最佳实践。
* **轻松的数据迁移**：使用我们的[prom-migrator](https://github.com/timescale/promscale/blob/master/migration-tool/cmd/prom-migrator/README.md) 工具轻松地将您现有的 Prometheus 数据迁移到 Promscale。
* **K8s 上的简化部署**：使用[tobs](https://github.com/timescale/tobs)部署和管理一个完整的、预配置的和生产就绪的可观察性堆栈，用于 K8s 集群上的指标和跟踪，包括 Promscale、Prometheus、具有自动检测的 OpenTelemetry、Grafana 和大量的外部-开箱即用的仪表板和警报。

### 架构说明

![](https://s2.loli.net/2023/03/21/AMUosrD8lhFCX72.png)


### [在 Linux 上安装 TimescaleDB](https://docs.timescale.com/install/latest/self-hosted/installation-linux/)
1. 在命令提示符下，以 root 身份添加 PostgreSQL 第三方存储库以获取最新的 PostgreSQL 包：
```powershell
sudo apt install gnupg postgresql-common apt-transport-https lsb-release wget -y
```
2. 运行 PostgreSQL 存储库设置脚本：
```powershell
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
```
3. 添加TimescaleDB第三方仓库：
```powershell
echo "deb https://packagecloud.io/timescale/timescaledb/ubuntu/ $(lsb_release -c -s) main" | sudo tee /etc/apt/sources.list.d/timescaledb.list
```
4. 安装时间刻度 GPG 密钥
```powershell
wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo apt-key add -
```
5. 更新软件列表
```powershell
sudo apt update
```
6. 安装TimescaleDB拓展:
```powershell
sudo apt install timescaledb-2-postgresql-15 -y
```
7. 安装 promscale拓展
```powershell 
sudo apt install promscale-extension-postgresql-15 -y
```

#### 使用 apt 包管理器安装 psql
1. 确保您的apt存储库是最新的：
```powershell
sudo apt update
```
2. 安装postgresql-client包：
```powershell
sudo apt install postgresql-client -y 
```

#### 在基于 Ubuntu 的系统上设置 TimescaleDB 扩展
1. 使用以下命令启用 TimescaleDB 后重新启动服务timescaledb-tune：
```powershell
sudo systemctl restart postgresql
```
2. 在您的本地系统上，在命令提示符下，`psql`以超级用户身份打开命令行实用程序`postgres`：
```powershell
sudo -u postgres psql
```
3. 如果连接成功，您将看到如下消息，然后是提示psql：
```powershell
psql (15.2 (Ubuntu 15.2-1.pgdg20.04+1))
Type "help" for help.
```
4. 为用户设置密码`postgres`：
```powershell
\password postgres
```
5. 退出 PostgreSQL：
```powershell
\q
```
6. 使用psql客户端连接到 PostgreSQL：
```powershell
psql -U postgres -h localhost
```
7. 在psql提示符下，创建一个空数据库。我们的数据库称为tsdb：
```powershell
CREATE database timescale;
```
退出进入到`tsdb`
```powershell
\q
psql -U postgres -h localhost -d timescale
```
8. 添加 TimescaleDB promscale 扩展：
```powershell
CREATE EXTENSION IF NOT EXISTS timescaledb; 
CREATE EXTENSION IF NOT EXISTS promscale; 
```
添加拓展成功会有以下提示：
```powershell
timescale=# CREATE EXTENSION IF NOT EXISTS timescaledb; 
WARNING:  
WELCOME TO
 _____ _                               _     ____________  
|_   _(_)                             | |    |  _  \ ___ \ 
  | |  _ _ __ ___   ___  ___  ___ __ _| | ___| | | | |_/ / 
  | | | |  _ ` _ \ / _ \/ __|/ __/ _` | |/ _ \ | | | ___ \ 
  | | | | | | | | |  __/\__ \ (_| (_| | |  __/ |/ /| |_/ /
  |_| |_|_| |_| |_|\___||___/\___\__,_|_|\___|___/ \____/
               Running version 2.10.1
For more information on TimescaleDB, please visit the following links:

 1. Getting started: https://docs.timescale.com/timescaledb/latest/getting-started
 2. API reference documentation: https://docs.timescale.com/api/latest
 3. How TimescaleDB is designed: https://docs.timescale.com/timescaledb/latest/overview/core-concepts

Note: TimescaleDB collects anonymous reports to better understand and assist our users.
For more information and how to disable, please see our docs https://docs.timescale.com/timescaledb/latest/how-to-guides/configuration/telemetry.

CREATE EXTENSION
```
如果提示安装失败可以输入以下命令解决：
```powershell
sudo echo "shared_preload_libraries = 'timescaledb'" >> /etc/postgresql/15/main/postgresql.conf
```
9. `\dx `在提示符处使用命令检查是否安装了 TimescaleDB 扩展psql。输出类似于：
```powershell
tsdb=# \dx
                                      List of installed extensions
    Name     | Version |   Schema   |                            Description                            
-------------+---------+------------+-------------------------------------------------------------------
 plpgsql     | 1.0     | pg_catalog | PL/pgSQL procedural language
 timescaledb | 2.10.1  | public     | Enables scalable inserts and complex queries for time-series data
(2 rows)

```
10. 创建扩展和数据库后，您可以使用以下命令直接连接到数据库：
```powershell
psql -U postgres -h localhost -d timescale
```

### 安装 Promscale 连接器
`Promscale `连接器本地使用 `PromQL` 查询并从 `TimescaleDB` 获取数据以执行它们，而 `SQL` 查询直接进入 `TimescaleDB`。安装 TimescaleDB 和 Promscale 扩展后，您可以使用以下命令安装 Promscale 连接器：
```powershell
wget https://github.com/timescale/promscale/releases/download/0.17.0/promscale_0.17.0_Linux_x86_64.deb
sudo dpkg -i promscale_0.17.0_Linux_x86_64.deb 

# 修改配置文件 /etc/promscale.conf
# PROMSCALE_METRICS_ASYNC_ACKS=""
PROMSCALE_DB_CONNECTIONS_MAX="-1"
PROMSCALE_DB_HOST="localhost"
PROMSCALE_DB_NAME="timescale"
PROMSCALE_DB_PASSWORD="8808"
PROMSCALE_DB_PORT="5432"
# PROMSCALE_DB_SSL_MODE="prefer"
PROMSCALE_DB_USER="postgres"
PROMSCALE_DB_NUM_WRITER_CONNECTIONS="4"
PROMSCALE_STARTUP_INSTALL_EXTENSIONS="true"
PROMSCALE_METRICS_CACHE_LABELS_SIZE="10000"
PROMSCALE_TELEMETRY_LOG_FORMAT="logfmt"
PROMSCALE_METRICS_CACHE_METRICS_SIZE="10000"
# PROMSCALE_TELEMETRY_LOG_THROUGHPUT_REPORT_INTERVAL=""
# PROMSCALE_STARTUP_USE_SCHEMA_VERSION_LEASE=""
PROMSCALE_WEB_CORS_ORIGIN=".*"
PROMSCALE_WEB_LISTEN_ADDRESS=":9201"
PROMSCALE_WEB_TELEMETRY_PATH="/metrics"


# 开机自启
sudo systemctl enable promscale
# 开始服务
sudo systemctl start promscale
# 停止服务
sudo systemctl stop promscale
# 服务状态
sudo systemctl status promscale
# 重启服务
sudo systemctl restart promscale
```

### TimescaleDB调优工具
为了帮助更轻松地配置 TimescaleDB，您可以使用该[timescaledb-tune](https://github.com/timescale/timescaledb-tune)工具。该工具可根据您的系统将最常见的参数设置为合适的值。可以使用以下go install命令安装它：
```powershell
go install github.com/timescale/timescaledb-tune/cmd/timescaledb-tune@latest
```
该timescaledb-tune工具读取您的系统`postgresql.conf`文件并为您的设置提供交互式建议。以下是该工具运行的示例：
```powershell
$ sudo timescaledb-tune 
Using postgresql.conf at this path:
/etc/postgresql/15/main/postgresql.conf

Is this correct? [(y)es/(n)o]: y
Writing backup to:
/tmp/timescaledb_tune.backup202303222058

success: shared_preload_libraries is set correctly

Tune memory/parallelism/WAL and other settings? [(y)es/(n)o]: y
Recommendations based on 31.22 GB of available memory and 12 CPUs for PostgreSQL 15

Memory settings recommendations
Current:
shared_buffers = 128MB
#effective_cache_size = 4GB
#maintenance_work_mem = 64MB
#work_mem = 4MB
Recommended:
shared_buffers = 7992MB
effective_cache_size = 23977MB
maintenance_work_mem = 2047MB
work_mem = 6820kB
Is this okay? [(y)es/(s)kip/(q)uit]: y
success: memory settings will be updated

Parallelism settings recommendations
Current:
missing: timescaledb.max_background_workers
#max_worker_processes = 8
#max_parallel_workers_per_gather = 2
#max_parallel_workers = 8
Recommended:
timescaledb.max_background_workers = 16
max_worker_processes = 31
max_parallel_workers_per_gather = 6
max_parallel_workers = 12
Is this okay? [(y)es/(s)kip/(q)uit]: y
success: parallelism settings will be updated

WAL settings recommendations
Current:
#wal_buffers = -1
min_wal_size = 80MB
Recommended:
wal_buffers = 16MB
min_wal_size = 512MB
Is this okay? [(y)es/(s)kip/(q)uit]: y
success: WAL settings will be updated

Background writer settings recommendations
Current:
Recommended:
Is this okay? [(y)es/(s)kip/(q)uit]: y
success: background writer settings will be updated

Miscellaneous settings recommendations
Current:
#default_statistics_target = 100
#random_page_cost = 4.0
#checkpoint_completion_target = 0.9
#max_locks_per_transaction = 64
#autovacuum_max_workers = 3
#autovacuum_naptime = 1min
#effective_io_concurrency = 1
Recommended:
default_statistics_target = 500
random_page_cost = 1.1
checkpoint_completion_target = 0.9
max_locks_per_transaction = 256
autovacuum_max_workers = 10
autovacuum_naptime = 10
effective_io_concurrency = 256
Is this okay? [(y)es/(s)kip/(q)uit]: y
success: miscellaneous settings will be updated
Saving changes to: /etc/postgresql/15/main/postgresql.conf
```
回答完问题后，更改将写入您的计算机 postgresql.conf并在您下次重新启动时生效。
```powershell
sudo systemctl restart postgresql@15-main.service 
```
如果您从一个新实例开始并且不想批准每组更改，您可以`postgresql.conf`在运行该工具时使用一些额外的标志自动接受建议并将其附加到您的末尾：
```powershell
timescaledb-tune --quiet --yes --dry-run >> /path/to/postgresql.conf
```

### 二进制部署 prometheus：
prometheus官网下载地址：[Download | Prometheus](https://prometheus.io/download/)
```powershell
# 创建prometheus目录
sudo mkdir -p /opt/monitor

# 下载
wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz

#解压移动
tar -xf prometheus-2.43.0.linux-amd64.tar.gz
sudo mv prometheus-2.43.0.linux-amd64 /opt/monitor/prometheus

# 添加systemctl管理
sudo vim /etc/systemd/system/prometheus.service
# 添加以下内容
[Unit]
Description=prometheus
[Service]
ExecStart=/opt/monitor/prometheus/prometheus --config.file=/opt/monitor/prometheus/prometheus.yml
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
[Install]
WantedBy=multi-user.target
# 保存退出

#启动prometheus
sudo systemctl daemon-reload 
sudo systemctl enable prometheus.service 
sudo systemctl start prometheus.service 
sudo systemctl status prometheus.service
```

### 二进制部署 node_exporter 指标采集器
prometheus采集器地址： [Exporters and integrations | Prometheus](https://prometheus.io/docs/instrumenting/exporters/)
```powershell
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
tar -xzvf node_exporter-1.5.0.linux-amd64.tar.gz
sudo mv node_exporter-1.5.0.linux-amd64 /opt/monitor/node_exporter

# 添加systemctl管理
sudo vim /etc/systemd/system/node_exporter.service
# 添加以下内容
[Unit]
Description=node_exporter
[Service]
ExecStart=/opt/monitor/node_exporter/node_exporter
#ExecStart=/opt/monitor/node_exporter/node_exporter --web.config=/opt/monitor/node_exporter/config.yml
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
[Install]
WantedBy=multi-user.target
# 保存退出

# 启动node_exporter
sudo systemctl daemon-reload 
sudo systemctl enable node_exporter.service 
sudo systemctl start node_exporter.service 
sudo systemctl status node_exporter.service 
```


### 二进制部署 granfana
granfana官网下载地址： [Download Grafana | Grafana Labs](https://grafana.com/grafana/download)
```powershell
# 下载最新版本
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-9.4.7.linux-amd64.tar.gz
tar -zxvf grafana-enterprise-9.4.7.linux-amd64.tar.gz
sudo mv grafana-9.4.7 /opt/monitor/grafana

# 添加systemctl管理
sudo vim /etc/systemd/system/grafana.service
# 添加以下内容
[Unit]
Description=grafana
[Service]
ExecStart=/opt/monitor/grafana/bin/grafana-server -homepath=/opt/monitor/grafana
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
[Install]
WantedBy=multi-user.target
# 保存退出

# 启动grafana
sudo systemctl daemon-reload 
sudo systemctl enable grafana.service
sudo systemctl start grafana.service
sudo systemctl status grafana.service 

```

### 配置 Prometheus 从 Promscale 读写数据
```powershell
# 修改 prometheus 配置文件
sudo vim /opt/monitor/prometheus/prometheus.yml
# 在配置文件最后添加以下内容：
remote_write:
   - url: "http://127.0.0.1:9201/write"
     write_relabel_configs:
     - source_labels: [__name__]
       regex: '.*:.*'
       action: drop
     remote_timeout: 100s
     queue_config:
       capacity: 500000
       max_samples_per_send: 50000
       batch_send_deadline: 30s
       min_backoff: 100ms
       max_backoff: 10s
       min_shards: 16
       max_shards: 16
remote_read:
  - url: "http://127.0.0.1:9201/read"
    read_recent: true
# 重启服务
sudo systemctl restart prometheus.service
# 查看日志
sudo systemctl status prometheus.service
```

### 验证采集的数据
进入 timescale 数据库中查询：
```powershell
psql -U postgres -h localhost -d timescale

timescale=# \d
                                        List of relations
   Schema    |                              Name                              | Type |   Owner    
-------------+----------------------------------------------------------------+------+------------
 prom_metric | go_gc_duration_seconds                                         | view | prom_admin
 prom_metric | go_gc_duration_seconds_count                                   | view | prom_admin
 prom_metric | go_gc_duration_seconds_sum                                     | view | prom_admin
 prom_metric | go_goroutines                                                  | view | prom_admin
 prom_metric | go_info                                                        | view | prom_admin
 prom_metric | go_memstats_alloc_bytes                                        | view | prom_admin
 prom_metric | go_memstats_alloc_bytes_total                                  | view | prom_admin
 prom_metric | go_memstats_buck_hash_sys_bytes                                | view | prom_admin
 prom_metric | go_memstats_frees_total                                        | view | prom_admin
 prom_metric | go_memstats_gc_sys_bytes                                       | view | prom_admin
 prom_metric | go_memstats_heap_alloc_bytes                                   | view | prom_admin
 prom_metric | go_memstats_heap_idle_bytes                                    | view | prom_admin
 prom_metric | go_memstats_heap_inuse_bytes                                   | view | prom_admin
 prom_metric | go_memstats_heap_objects                                       | view | prom_admin
 prom_metric | go_memstats_heap_released_bytes                                | view | prom_admin
 prom_metric | go_memstats_heap_sys_bytes                                     | view | prom_admin
 prom_metric | go_memstats_last_gc_time_seconds                               | view | prom_admin
 prom_metric | go_memstats_lookups_total                                      | view | prom_admin
 prom_metric | go_memstats_mallocs_total                                      | view | prom_admin
 prom_metric | go_memstats_mcache_inuse_bytes                                 | view | prom_admin
 prom_metric | go_memstats_mcache_sys_bytes                                   | view | prom_admin
 prom_metric | go_memstats_mspan_inuse_bytes                                  | view | prom_admin
 prom_metric | go_memstats_mspan_sys_bytes                                    | view | prom_admin
 prom_metric | go_memstats_next_gc_bytes                                      | view | prom_admin
 prom_metric | go_memstats_other_sys_bytes                                    | view | prom_admin
 prom_metric | go_memstats_stack_inuse_bytes                                  | view | prom_admin
 prom_metric | go_memstats_stack_sys_bytes                                    | view | prom_admin
 prom_metric | go_memstats_sys_bytes                                          | view | prom_admin
 prom_metric | go_threads                                                     | view | prom_admin
 prom_metric | net_conntrack_dialer_conn_attempted_total                      | view | prom_admin
 prom_metric | net_conntrack_dialer_conn_closed_total                         | view | prom_admin
 prom_metric | net_conntrack_dialer_conn_established_total                    | view | prom_admin
 prom_metric | net_conntrack_dialer_conn_failed_total                         | view | prom_admin
 prom_metric | net_conntrack_listener_conn_accepted_total                     | view | prom_admin
 prom_metric | net_conntrack_listener_conn_closed_total                       | view | prom_admin
 prom_metric | node_arp_entries                                               | view | prom_admin
 prom_metric | node_boot_time_seconds                                         | view | prom_admin
 prom_metric | node_btrfs_allocation_ratio                                    | view | prom_admin
 prom_metric | node_btrfs_device_size_bytes                                   | view | prom_admin
.......
# 可以看到生成了很多数据指标的表

# 查询一个过去五分钟 io 的指标
timescale-# SELECT * from node_disk_io_now WHERE time > now() - INTERVAL '5 minutes';
            time            | value | series_id |      labels       | device_id | instance_id | job_id 
----------------------------+-------+-----------+-------------------+-----------+-------------+--------
 2023-03-22 22:43:52.98+08  |     0 |      2532 | {151,601,517,520} |       601 |         517 |    520
 2023-03-22 22:44:07.98+08  |     0 |      2532 | {151,601,517,520} |       601 |         517 |    520
 2023-03-22 22:43:52.98+08  |     0 |      2565 | {151,619,517,520} |       619 |         517 |    520
 2023-03-22 22:44:07.98+08  |     0 |      2565 | {151,619,517,520} |       619 |         517 |    520
 2023-03-22 22:43:52.98+08  |     0 |      2595 | {151,636,517,520} |       636 |         517 |    520
 2023-03-22 22:44:07.98+08  |     0 |      2595 | {151,636,517,520} |       636 |         517 |    520
 2023-03-22 22:44:07.98+08  |     0 |      2546 | {151,609,517,520} |       609 |         517 |    520
 2023-03-22 22:44:22.98+08  |     0 |      2546 | {151,609,517,520} |       609 |         517 |    520
 2023-03-22 22:44:07.98+08  |     0 |      2579 | {151,627,517,520} |       627 |         517 |    520
 2023-03-22 22:44:22.98+08  |     0 |      2579 | {151,627,517,520} |       627 |         517 |    520
 2023-03-22 22:44:07.98+08  |     0 |      2540 | {151,606,517,520} |       606 |         517 |    520
...

# 例如，要查找指标的中值node_disk_io_now，按与其关联的工作分组：
timescale-# SELECT
    val(job_id) as job,
    percentile_cont(0.5) within group (order by value) AS median
FROM
    node_disk_io_now
WHERE
    time > now() - INTERVAL '5 minutes'
GROUP BY job_id;

    job    | median 
-----------+--------
 localhost |      0
 nas-node  |      0
(2 rows)

# 任何度量行中的labels字段表示与测量相关的完整标签集。它表示为标识符数组。要以 JSON 格式返回整个标签集，你可以使用该jsonb()函数，如下所示：
timescale=# SELECT
    time, value, jsonb(labels) as labels
FROM
    node_disk_io_now
WHERE
    time > now() - INTERVAL '5 minutes';

            time            | value |                                                  labels                                                  
----------------------------+-------+----------------------------------------------------------------------------------------------------------
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "md1", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:52.98+08  |     0 | {"job": "nas-node", "device": "md1", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:32.003+08 |     0 | {"job": "localhost", "device": "nvme0n1", "__name__": "node_disk_io_now", "instance": "localhost:9100"}
 2023-03-22 22:46:47.003+08 |     0 | {"job": "localhost", "device": "nvme0n1", "__name__": "node_disk_io_now", "instance": "localhost:9100"}
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "sata1p1", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:52.98+08  |     0 | {"job": "nas-node", "device": "sata1p1", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "sata4p2", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:52.98+08  |     0 | {"job": "nas-node", "device": "sata4p2", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "synoboot1", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:52.98+08  |     0 | {"job": "nas-node", "device": "synoboot1", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "dm-2", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:52.98+08  |     0 | {"job": "nas-node", "device": "dm-2", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "sata1p2", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:52.98+08  |     0 | {"job": "nas-node", "device": "sata1p2", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "sata3", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:52.98+08  |     0 | {"job": "nas-node", "device": "sata3", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
 2023-03-22 22:46:37.98+08  |     0 | {"job": "nas-node", "device": "synoboot2", "__name__": "node_disk_io_now", "instance": "10.0.0.5:9100"}
```
或者使用客户端查看指标数据，这里使用 dbeaver-ce 为例

![](https://s2.loli.net/2023/03/22/hSqsJrpewUv4OtX.png)

以上是使用 Promscale 存储 Prometheus 的指标数据到 psql。