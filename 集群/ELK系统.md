# Elasticsearch + Logstash + Kibana  [ELK系统]

* 2023-03-16

### 介绍
Elasticsearch 是一个实时的分布式搜索分析引擎，它能让你以前所未有的速度和规模，去探索你的数据。 它被用作全文检索、结构化搜索、分析以及这三个功能的组合：

* Wikipedia 使用 Elasticsearch 提供带有高亮片段的全文搜索，还有 search-as-you-type 和 did-you-mean 的建议。
* 卫报 使用 Elasticsearch 将网络社交数据结合到访客日志中，为它的编辑们提供公众对于新文章的实时反馈。
* Stack Overflow 将地理位置查询融入全文检索中去，并且使用 more-like-this 接口去查找相关的问题和回答。
* GitHub 使用 Elasticsearch 对1300亿行代码进行查询。

[Elastic Stack 兼容性之 ES 与 JDK：JDK版本兼容性及版本推荐](https://www.elastic.org.cn/archives/elasticjdk)

### 分片与副本
Elasticsearch 将数据分片（Sharding）和复制（Replication）以提供可靠性和扩展性。具体地说，分片将数据分散到不同的节点上，而副本在这些节点之间复制数据，以确保数据的冗余备份和快速恢复。

* 分片（Sharding）指将索引分割成多个分片，每个分片存储一部分数据，并在不同的节点上进行分发。这样做的好处是可以将一个大的索引分成多个小的分片，方便存储和处理，并且可以实现水平扩展，提高吞吐量和查询性能。
* 副本（Replication）指将分片复制到其他节点上，以提高数据的可靠性。每个副本都可以被用作读取请求的目标，因此可以提高读取性能。副本的数量可以在索引级别配置，通常是设置为一个大于等于1的整数，可以根据需要进行更改。
* 总的来说，分片和副本是 Elasticsearch 分布式架构的核心组成部分，它们能够提供高可用性、高性能和可靠性。

### 安装并运行 Elasticsearch
安装 Elasticsearch 之前，需要先安装一个较新的版本的 Java，最好的选择是，你可以从 [Java](https://www.java.com) 获得官方提供的最新版本的 Java,但是由于这次使用的是`Elasticsearch 7.x`版本，所以java版本为8或者jdk1.8就好了，Ubuntu20 系统是自带java8的，所以直接下载Elasticsearch 安装包就好了，如果使用`Elasticsearch 8.x`请使用java 17,下面是升级为java 17的步骤。
以Ubuntu为例，可以直接在apt进行安装：

```powershell
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
```

从 [elastic 的官网](https://elastic.co/downloads/elasticsearch)获取最新版本的 Elasticsearch。要想安装 Elasticsearch，先下载并解压适合你操作系统的 Elasticsearch 版本。

以下使用deb安装包安装

```powershell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.9-amd64.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.9-amd64.deb.sha512
shasum -a 512 -c elasticsearch-7.17.9-amd64.deb.sha512 
sudo dpkg -i elasticsearch-7.17.9-amd64.deb
# -- 安装成功提示
### NOT starting on installation, please execute the following statements to configure elasticsearch service to start automatically using systemd
 sudo systemctl daemon-reload
 sudo systemctl enable elasticsearch.service
### You can start elasticsearch service by executing
 sudo systemctl start elasticsearch.service
Created elasticsearch keystore in /etc/elasticsearch/elasticsearch.keystore
Processing triggers for systemd (245.4-4ubuntu3.20) ...

# 完全卸载
# 删除以前版本的ElasticSearch：
sudo apt-get --purge autoremove elasticsearch
# 删除ElasticSearch目录：
sudo rm -rf /var/lib/elasticsearch/
sudo rm -rf /etc/elasticsearch
```

使用systemd运行 Elasticsearch 
```powershell
#  注意启动前请设置jvm的运行大小，默认推荐的是系统内存的一半，这样启动电脑会直接卡死，所以需要修改
sudo vim /etc/elasticsearch/jvm.options
# 把
## -Xms4g
## -Xmx4g
# 改成
-Xms1g
-Xmx1g
# 保存退出

# 修改elasticsearch.yml文件
sudo vim /etc/elasticsearch/elasticsearch.yml
# 在 elasticsearch.yml中末尾加入：
http.cors.enabled: true
http.cors.allow-origin: "*"
network.host: 127.0.0.1
# 目的是使ES支持跨域请求
# 保存退出

# 最后打开服务
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service # 设置开机启动
# 开启/停止
sudo systemctl start elasticsearch.service  # 开启服务
sudo systemctl stop elasticsearch.service   # 停止服务
sudo systemctl restart elasticsearch.service  # 重启服务
sudo systemctl status elasticsearch.service # 查看服务
```

检查 Elasticsearch 是否运行:
```powershell
# 可以通过向以下端口发送 HTTPS 请求来测试您的 Elasticsearch 节点是否正在运行 localhost：9200
curl 'http://localhost:9200/?pretty'

# 终端输出
{
  "name" : "master",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "H_fF24IRQ_ODIOXVIOW4Zg",
  "version" : {
    "number" : "7.17.9",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "ef48222227ee6b9e70e502f0f0daa52435ee634d",
    "build_date" : "2023-01-31T05:34:43.305517834Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
# 看到终端打印出这些信息就说明服务启动成功了
```
或者打开浏览器输入： [http://localhost:9200/](http://localhost:9200/) 也可以看到以上的文本既可说明启动成功！

### Kibana的安装与使用
#### Kibana介绍
* 一个针对Elasticsearch的开源分析及可视化平台
* 搜索、查看存储在Elasticsearch索引中的数据
* 通过各种图表进行高级数据分析及展示

#### Kibana主要功能
* Elasticsearch无缝之集成
* 整合数据，复杂数据分析
* 让更多团队成员受益
* 接口灵活，分享更容易
* 配置简单，可视化多数据源
* 简单数据导出

请到[kibana-releases](https://www.elastic.co/cn/downloads/past-releases#kibana)找到与Elasticsearch 对应版本的进行下载，你可以点击[下载链接](https://artifacts.elastic.co/downloads/kibana/kibana-7.17.9-linux-x86_64.tar.gz)进行下载。

```powershell
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.17.9-amd64.deb
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.17.9-amd64.deb.sha512
shasum -a 512 -c kibana-7.17.9-amd64.deb.sha512
sudo dpkg -i kibana-7.17.9-amd64.deb

# 开始Kibana服务
sudo systemctl daemon-reload
sudo systemctl enable kibana.service  # 设置开机启动
# 开启/停止
sudo systemctl start kibana.service  # 开启服务
sudo systemctl stop kibana.service    # 停止服务
sudo systemctl restart kibana.service # 重启服务
sudo systemctl status kibana.service  # 查看服务
```
打开浏览器输入：[http://localhost:5601/](http://localhost:5601/)，可以看到Kibana的界面：

![](https://s2.loli.net/2023/03/14/ScKv7GmbT4lfirj.png)

里面可以添加一些样例数据进行查看，并生成面板，下面以网站log数据分析为例：

![](https://s2.loli.net/2023/03/14/hwfPm8UpnGAIWkx.png)

关闭 Kibana服务
```powershell
ps -ef | grep 5601
# or
ps -ef | grep kibana
# or
lsof -i:5601

# 找到对应的pid ，并杀死pid
kill -9 $pid
```

**关于“Kibana server is not ready yet'”问题的原因及解决办法**
* Kibana和Elasticsearch的版本不兼容。
>> 解决办法：保持版本一直
* Elasticsearch的服务地址和Kibana中配置的elasticsearch.hosts:不同
>> 解决办法：修改kibana.yml中的elasticsearch.hosts配置
* Elasticsearch中禁止跨域访问
>> 解决办法：在elasticsearch.yml中配置允许跨域
* 服务器中开启了防火墙
>> 解决办法：关闭防火墙或者修改服务器的安全策略
* Elasticsearch所在磁盘剩余空间不足**90%**
>> 解决办法：清理磁盘空间，配置监控和报警

### Head插件的安装,可视化工具，方便查看集群信息并管理集群
1. 安装nodejs，ubuntu系统是自带的，不过版本会比较老需要自己更新一下。
2. 安装`grunt`
```powershell
sudo npm install -g grunt-cli
grunt -version
```
3. 下载 Head 插件
   1. 下载地址：https://github.com/mobz/elasticsearch-head
    ```powershell
    wget https://github.com/mobz/elasticsearch-head/archive/refs/tags/v5.0.0.tar.gz
    tar -xvzf v5.0.0.tar.gz
    cd elasticsearch-head
    ```
   2.  修改`Gruntfile.js`，添加`hostname：'*'`,如下图所示：

    ![](https://s2.loli.net/2023/03/14/yz1Wkv7dFXxq9Qb.png)

   3. 安装以来`npm install`
   4. 启动服务`npm run start` 
   5. 打开浏览器输入：[http://localhost:9100](http://localhost:9100)，看到以下界面代表安装成功;

    ![](https://s2.loli.net/2023/03/14/BW5rZqpVbiwAhF2.png)


### ES 集群参数的查询
使用Kibana的 Dev Tools可以对ES集群进行状态参数的查询，回到Kibana的首页，点击`Dev Tools`

![](https://s2.loli.net/2023/03/15/YPlionVeXkUbS5y.png)

在左边输入栏输入以下指令，点击`三角形`运行按钮即可对ES集群进行查询：
```powershell
# 查询集群的健康值，加上?v可以查看对应的值

Get _cat/health?v

# 查询集群的健康值以Json的格式返回

Get _cluster/health
```

![](https://s2.loli.net/2023/03/15/PiZKqtFsBbyWYNw.png)

![](https://s2.loli.net/2023/03/15/IOgiz9wbjZRexku.png)


### 安装Logstash
前面我们安装的Elasticsearch版本是7.17.9，所以Logstash和接下来要安装的Kibana都要安装7.17.9这个版本。

```powershell
wget https://artifacts.elastic.co/downloads/logstash/logstash-7.17.9-amd64.deb
wget https://artifacts.elastic.co/downloads/logstash/logstash-7.17.9-amd64.deb.sha512
shasum -a 512 -c logstash-7.17.9-amd64.deb.sha512
sudo dpkg -i logstash-7.17.9-amd64.deb

# 修改logstash的配置
sudo vim /etc/logstash/logstash-sample.conf 

# 把input部分的先注释
# input {
#   beats {
#     port => 5044
#   }
# }
# 添加以下配置
input {
  tcp {
    # Logstash 作为服务
    mode => "server"
    host => "localhost"
    # 开放9001端口进行采集日志
    port => 9001
    # 编解码器
    codec => json_lines
  }
}
# 保存退出


# 开始Logstash服务
sudo systemctl daemon-reload
sudo systemctl enable logstash.service   # 设置开机启动
# 开启/停止
sudo systemctl start logstash.service  # 开启服务
sudo systemctl stop logstash.service    # 停止服务
sudo systemctl restart logstash.service # 重启服务
sudo systemctl status logstash.service   # 查看服务
```

### Logstash 配置参数说明

输入插件通用参数

| 参数名        | 参数类型 | 默认值 | 说明                                                                               |
|---------------|----------|--------|------------------------------------------------------------------------------------|
| add_field     | hash     | {}     | 添加一个字段到一个事件，放到时间顶部，一般用于标记日志来源。如属于哪个项目哪个应用 |
| codec         | codec    | line   | 编码器                                                                             |
| enable_metric | boolean  | true   | 是否开启指标日志                                                                   |
| id            | string   | \\     | 插件ID，未设置时Logstash自动生成                                                   |
| tags          | array    | \\     | 添加任意数量的标签，用于标记日志的其他属性，如表明访问日志还是错误日志             |
| type          | string   | \\     | 为所有输入添加一个字段，如表明日志类型                                             |

输出插件通用参数

| 参数名称      | 参数类型 | 默认值    | 说明                                      |
|---------------|----------|-----------|-------------------------------------------|
| codec         | codc     | rubydebug | 编解码器，除了rubydebug,还有plain或者line |
| enable_metric | boolean  | trun      | 是否开启指标日志                          |
| id            | string   | \\        | 插件id，未设置时Logstash自动生成          |

过滤器通用参数

| 参数名称      | 参数类型 | 默认值 | 说明                                                        |
|---------------|----------|--------|-------------------------------------------------------------|
| add_field     | hash     | {}     | 如果过滤成功，添加一个字段到这个事件，可使用%{}的格式       |
| add_tag       | array    | []     | 如果过滤成功，添加任意数量的标签到这个事件，可使用%{}的格式 |
| enable_metric | boolean  | true   | 是否开启指标日志                                            |
| id            | string   | \\     | 过滤器ID，未指定时由Logstash自动分配                        |
| priodic_flush | boolean  | false  | 调用过滤器flush方法的时间间隔                               |
| remove_field  | array    | []     | 如果过滤成功，从这个时间移除任意字段，可以使用%{}的格式     |
| remove_tag    | array    | []     | 如果过滤成功，从这个时间移除任意字段，可以使用%{}的格式     |

File 插件
* 用于读取指定文件的内容
* 常用字段：
  * path: 文件路径
  * exclude: 排除采集的文件
  * start_position: 指定从文件什么位置开始读取，默认从结尾开始，指定beginning表示从头开始读
  * tags: 采集文件的标签
  * type: 采集文件的类型

### 使用Logstash对接系统日志
对接系统日志，需要在 Logstash 的配置文件`conf.d`中进行添加配置：
```powershell
# 收集系统日志前需要修改下系统日志的权限，不然会报拒绝访问
sudo chmod 644 /var/log/syslog

# 编辑配置文件
sudo vim /etc/logstash/conf.d/system.conf

# 添加以下内容：
input {
    file{
        path => "/var/log/syslog"
        type => "system.log"   #系统类型
        start_position => "beginning"
    }
}

output {
    elasticsearch {
        hosts => ["http://localhost:9200"]
        index => "system-%{+YYYY.MM.dd}"
    }
}

# 保存退出
# 重启logstash服务
sudo systemctl restart logstash.service
```
打开浏览器输入：[http://localhost:9100/](http://localhost:9100/)，点击`数据浏览`可以看到`system-2023-xx.xx`

![](https://s2.loli.net/2023/03/15/DpQZEvHA2Ls71Ng.png)

使用 Kibana 的 Dev Tools进行日志查询,返回的是Json格式的数据：输入`Get system-2023.03.15/_search`

![](https://s2.loli.net/2023/03/15/ZVYyGDCnOdU12Si.png)

### 使用Logstash对接 apache2 日志
对接系统日志，需要在 Logstash 的配置文件`conf.d`中进行添加配置：
```powershell
# 为了不修改日志的权限，所以需要把执行用户添加日志组里面
sudo usermod -a -G adm logstash

# 可以在上面的基础上添加以下内容：
input {
    file{
        path => "/var/log/apache2/*.log"
        exclude => "error.log"
        tags => ["web", "apache2"]
        type => "apache2-access.log"   
        start_position => "beginning"
        add_field => {
            "project" => "apache2-access.log"
            "app" => "apache2"
        }
    }
    file{
        path => "/var/log/syslog"
        type => "system.log"   
        start_position => "beginning"
      }

}

output {
    if [type] == "apache2-access.log" {
        elasticsearch {
            hosts => ["http://localhost:9200"]
            index => "apache2-%{+YYYY.MM.dd}"
        }
    }
    if [type] == "system.log" {
        elasticsearch {
            hosts => ["http://localhost:9200"]
            index => "system-%{+YYYY.MM.dd}"
        }    
    }
}

# 保存退出
# 重启服务
sudo systemctl restart logstash.service
```
打开浏览器输入：[http://localhost:9100/](http://localhost:9100/)，点击`数据浏览`可以看到`apache2-2023-xx.xx`

![](https://s2.loli.net/2023/03/16/igPNtAoywTr48XY.png)

使用 Kibana 的 Dev Tools进行日志查询：输入`Get apache2-2023.03.16/_search`

![](https://s2.loli.net/2023/03/16/CBYqPctWhNIoLHl.png)

使用Kibana的索引来对日志信息分析：点击Kibana的首页左上角的`三`，选择`Stack Management`

![](https://s2.loli.net/2023/03/16/glEX5teA398BjHV.png)

选择 Kibana 下的 `Index Patterns`

![](https://s2.loli.net/2023/03/16/MakcsfrDG41Rdoy.png)

点击`Create index pattern`,按照如下设置：

![](https://s2.loli.net/2023/03/16/tlMjhBS3kpbw26g.png)

成功如下所示：

![](https://s2.loli.net/2023/03/16/tOWfRJy3ehZxcjd.png)

点击 Analytics 下的 `Discover`，选择`apache2-*`即可展示日志的信息，由于apahce2的日志没有做Json格式化，所以过滤的字段比较有限

![](https://s2.loli.net/2023/03/16/mI4fY59B13Dnh8K.png)

也可以把使用面板来展示日志的信息，点击Analytics 下的 `Dashboard` ，点击 `Create visualization`

![](https://s2.loli.net/2023/03/16/i29StIQU38E1RTe.png)

创建一个apache2服务时间点击数的图表，如下设置：

![](https://s2.loli.net/2023/03/16/tUyRQSbqhovaEnG.png)

输入图表的标题和描述

![](https://s2.loli.net/2023/03/16/G1tQWypqfhLzOPu.png)

最好展示在面板上

![](https://s2.loli.net/2023/03/16/tYdKPk7QT2GxpUh.png)








