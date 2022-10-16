# Docker 学习笔记

* 2022-06-10

### 参考Github的入门教程
* 链接：https://github.com/jaywcjlove/docker-tutorial
* Docker学习思维导图：

![](https://s2.loli.net/2022/06/08/Sc12dqDeaBmGFxY.png)

* 相关链接：
  * https://github.com/eon01/DockerCheatSheet
  * http://www.dockerinfo.net/document 
  * https://github.com/llitfkitfk/docker-tutorial-cn

## Docker简介
Docker 是一个开源的应用容器引擎，而一个容器containers其实是一个虚拟化的独立的环境，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

Docker 的局限性之一，它只能用在 64 位的操作系统上。

### 定义：
Docker是一个虚拟环境容器，可以将你的开发环境、代码、配置文件等一并打包到这个容器中，并发布和应用到任意平台中。 原理： docker底层使用了LXC来实现，LXC将linux进程沙盒化，使得进程之间相互隔离，并且能够协调各进程的资源分配。 LXC： LXC为Linux Container的简写。可以提供轻量级的虚拟化，以便隔离进程和资源，而且不需要提供指令解释机制以及全虚拟化的其他复杂性。

### Docker三个重要概念：
* 镜像（Image）：类似于虚拟机中的镜像，是一个包含有文件系统的面向Docker引擎的只读模板。任何应用程序运行都需要环境，而镜像就是用来提供这种运行环境的。 
* 容器（Container）：类似于一个轻量级的沙盒，可以将其看作一个极简的Linux系统环境（包括root权限、进程空间、用户空间和网络空间等），以及运行在其中的应用程序。Docker引擎利用容器来运行、隔离各个应用。容器是镜像创建的应用实例，可以创建、启动、停止、删除容器，各个容器之间是是相互隔离的，互不影响。注意：镜像本身是只读的，容器从镜像启动时，Docker在镜像的上层创建一个可写层，镜像本身不变。 
* 仓库（Repository）：类似于代码仓库，这里是镜像仓库，是Docker用来集中存放镜像文件的地方。注意与注册服务器（Registry）的区别：注册服务器是存放仓库的地方，一般会有多个仓库；而仓库是存放镜像的地方，一般每个仓库存放一类镜像，每个镜像利用tag进行区分。

### 容器和虚拟机的区别：
* 容器是应用程序层的抽象，将代码和依赖项打包在一起。多个容器可以在同一台计算机上运行，并与其他容器共享OS内核，每个容器在用户空间中作为隔离的进程运行。容器占用的空间少于VM（容器映像的大小通常为几十MB），可以处理更多的应用程序，并且需要的VM和操作系统更少。
* 虚拟机（VM）是将一台服务器转变为多台服务器的物理硬件的抽象。系统管理程序允许多个VM在单台计算机上运行。每个VM包含操作系统，应用程序，必要的二进制文件和库的完整副本-占用数十GB。VM也可能启动缓慢。
* 容器不是一台机器。Docker 利用的是 Linux 的资源分离机制，例如 cgroups，以及 Linux 核心命名空间（namespaces），来建立独立的容器（containers）。容器看上去是一台机器，实际上是一个进程。
* 相比于虚拟机，容器的优势主要有：
  * 资源占用少
  * 启动速度快
  * 本身体积小

|            | Docker容器  | LXC         | VM         |
| ---------- | ----------- | ----------- | ---------- |
| 虚拟机类型 | OS虚拟化    | OS虚拟化    | 硬件虚拟化 |
| 性能       | =物理机性能 | =物理机性能 | 5%-20%损耗 |
| 隔离性     | NS隔离      | NS隔离      | 强         |
| QoS        | Cgroup弱    | Cgroup弱    | 强         |
| 安全性     | 中          | 差          | 强         |
| GuestOS    | 全部        | 只支持Linux | 全部       |

## 安装并运行第一个Docker镜像
* 在Ubuntu系统安装Docker驱动：[官方教程](https://docs.docker.com/engine/install/ubuntu/)
* 卸载旧版本

```powershell
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

`apt-get`如果报告没有安装这些软件包，那也没关系。

### 使用存储库安装
在新主机上首次安装 Docker Engine 之前，您需要设置 Docker 存储库。之后，您可以从存储库安装和更新 Docker。

1. 更新apt包索引并安装包以允许apt通过 HTTPS 使用存储库：

```powershell
 $ sudo apt-get update
 $ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

2. 添加 Docker 的官方 GPG 密钥：

```powershell
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
3. 使用以下命令设置存储库：

```powershell
$  echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
### 安装 Docker 引擎
1. 更新apt包索引，安装最新版本的 Docker Engine、containerd 和 Docker Compose，或者进入下一步安装特定版本：

```powershell
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
2. 要安装特定版本的 Docker Engine，请在 repo 中列出可用版本，然后选择并安装：
一个。列出您的存储库中可用的版本：

```powershell
$ apt-cache madison docker-ce
 docker-ce | 5:20.10.17~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.16~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.15~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.14~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```

使用第二列中的版本字符串安装特定版本，例如`5:20.10.17~3-0~ubuntu-focal`

```powershell
$  sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin

# 安装5:20.10.17~3-0~ubuntu-focal
$ sudo apt-get install docker-ce=5:20.10.17~3-0~ubuntu-focal docker-ce-cli=5:20.10.17~3-0~ubuntu-focal containerd.io docker-compose-plugin
```
3. `hello-world` 通过运行映像来验证 Docker 引擎是否已正确安装。

```powershell
$ sudo docker run hello-world
```

结果如下：

```powershell
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:80f31da1ac7b312ba29d65080fddf797dd76acfb870e677f390d5acba9741b17
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### 卸载 Docker 引擎
1. 卸载 Docker Engine、CLI、Containerd 和 Docker Compose 软件包：

```powershell
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
2. 主机上的映像、容器、卷或自定义配置文件不会自动删除。要删除所有映像、容器和卷：

```powershell
$  sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```

您必须手动删除任何已编辑的配置文件。

### 设置非root用户不用`sudo`运行docker命令
1. 创建名为docker的组，如果之前已经有该组就会报错，可以忽略这个错误：

```powershell
$ sudo groupadd docker
```
2. 将当前用户加入组docker：

```shell
$ sudo gpasswd -a ${USER} docker
```

3. 重新注销用户再登录，打开终端运行docker命令可以不用加`sudo`了。


## Docker命令
* [docker命令详解](https://segmentfault.com/a/1190000008876540)
* [官网命令文档](https://docs.docker.com/engine/reference/commandline/)
### 帮助命令

```powershell
$ docker version        # 显示docker的版本信息
$ docker info           # 显示docker系统信息，包括镜像和容器的数量
$ docker 命令 --help    # 帮助命令
```

### 镜像命令
`docker images`查看所有本地主机上的镜像，[docker images命令](https://docs.docker.com/engine/reference/commandline/images/)

```powershell
$ docker images
REPOSITORY        TAG       IMAGE ID       CREATED        SIZE
new_dingbot_msg   latest    cde233707f86   14 hours ago   437MB
dingmsg_service   latest    269caf0cc7eb   21 hours ago   437MB
ubuntu            20.04     20fffa419e3a   3 days ago     72.8MB
httpd             latest    98f93cd0ec3b   13 days ago    144MB
hello-world       latest    feb5d9fea6a5   8 months ago   13.3kB

# 解释
REPOSITORY    镜像的仓库源
TAG           镜像的标签
IMAGE ID      镜像的id
CREATED       镜像的创建时间
SIZE          镜像的大小

# 可选项
  -a, --all             # 显示所有镜像
  -q, --quiet           # 显示镜像的id

```

`docker search`搜索镜像，[docker search](https://docs.docker.com/engine/reference/commandline/search/)

```powershell
$ docker search mysql
NAME                           DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                          MySQL is a widely used, open-source relation…   12708     [OK]       
mariadb                        MariaDB Server is a high performing open sou…   4878      [OK]       
percona                        Percona Server is a fork of the MySQL relati…   579       [OK]       
phpmyadmin                     phpMyAdmin - A web interface for MySQL and M…   553       [OK]       
bitnami/mysql                  Bitnami MySQL Docker Image                      71                   [OK]

# 可选项,通过搜索来过滤
--filter , -f=STARS=3000   #搜索镜像的STARS大于3000的
$ docker search mysql -f=stars=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   12708     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4878      [OK]   
```

`docker pull`下载镜像,[docker pull](https://docs.docker.com/engine/reference/commandline/pull/)

```powershell
# 下载镜像 docker pull 镜像名[:tag]
$ docker pull mysql
Using default tag: latest           # 如果不写tag，默认的是latest
latest: Pulling from library/mysql  
c1ad9731b2c7: Pull complete         # 分层下载 docker image 的核心 联合文件系统
54f6eb0ee84d: Pull complete 
cffcf8691bc5: Pull complete 
89a783b5ac8a: Pull complete 
6a8393c7be5f: Pull complete 
af768d0b181e: Pull complete 
810d6aaaf54a: Pull complete 
2e014a8ae4c9: Pull complete 
a821425a3341: Pull complete 
3a10c2652132: Pull complete 
4419638feac4: Pull complete 
681aeed97dfe: Pull complete 
Digest: sha256:548da4c67fd8a71908f17c308b8ddb098acf5191d3d7694e56801c6a8b2072cc  # 签名信息
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 真实地址

# 等价于
$ docke pull mysql
$ docke pull docker.io/library/mysql:latest

# 指定版本下载
$ docker pull mysql:5.7
5.7: Pulling from library/mysql
c1ad9731b2c7: Already exists      # 之前下载过的镜像是可以共用的，大大节省内存 联合文件系统最高明之处
54f6eb0ee84d: Already exists 
cffcf8691bc5: Already exists 
89a783b5ac8a: Already exists 
6a8393c7be5f: Already exists 
af768d0b181e: Already exists 
810d6aaaf54a: Already exists 
81fe6daf2395: Pull complete 
5ccf426818fd: Pull complete 
68b838b06054: Pull complete 
1b606c4f93df: Pull complete 
Digest: sha256:7e99b2b8d5bca914ef31059858210f57b009c40375d647f0d4d65ecd01d6b1d5
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7

# 查看镜像
$ docker images
REPOSITORY        TAG       IMAGE ID       CREATED        SIZE
mysql             5.7       2a0961b7de03   13 days ago    462MB
mysql             latest    65b636d5542b   13 days ago    524MB
```

`docker rmi`删除镜像，[docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/)

```shell
$ docker rmi -f 容器id    # 删除指定的容器
$ docker rmi -f 容器id 容器id 容器id 容器id 容器id # 删除多个容器
$ docker rmi -f $(docker images -aq) # 删除全部容器
```
### 容器命令
说明：我们有了镜像才能创建容器，下载centos镜像来测试

```powershell
$ docker pull centos
```
新建容器并启动,[docker run](https://docs.docker.com/engine/reference/commandline/run/)

```powershell
$ docker run [可选参数] image

# 参数选项
--name "Name"   # 容器名字 centos1 centos2 用于区分容器
-d              # 后台运行方式
-it             # 使用交互方式执行，进入容器查看内容
-p              # 指定容器端口  -p 8080:80
    -p ip:主机端口:容器端口
    -p 主机端口:容器端口(常用)
    -p 容器端口
    容器端口
-p              # 随机指定端口3

# 测试，启动进入容器
$ docker run -it centos /bin/bash
[root@f766035e7769 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

# 从容器退回主机
$ [root@f766035e7769 /]# exit
exit
```

* `docker ps`列出所有运行的容器

```powershell
$ docker ps 命令
    # 列出当前真在运行的的容器
-a    # 列出当前正在运行的容器+历史运行的容器
-n=?  # 显示最近创建的容器
-q    # 显示容器的编号

# 列出当前真在运行的的容器
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED        STATUS        PORTS                                       NAMES
7e61c738bcf4   dingmsg_service   "python3 app/lemumsg…"   22 hours ago   Up 22 hours   0.0.0.0:6655->6655/tcp, :::6655->6655/tcp   dingbot-test
986f882220ef   httpd             "httpd-foreground"       40 hours ago   Up 17 hours   0.0.0.0:8877->80/tcp, :::8877->80/tcp       apache-test

# 列出当前正在运行的容器+历史运行的容器
$ docker ps -a
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS                      PORTS                                       NAMES
f766035e7769   centos            "/bin/bash"              20 minutes ago   Exited (0) 5 minutes ago                                                beautiful_proskuriakova
7e61c738bcf4   dingmsg_service   "python3 app/lemumsg…"   22 hours ago     Up 22 hours                 0.0.0.0:6655->6655/tcp, :::6655->6655/tcp   dingbot-test
286484b695e4   20fffa419e3a      "/bin/sh -c 'apt-get…"   25 hours ago     Exited (127) 25 hours ago                                               nervous_kilby

# 显示最近创建的容器
$ docker ps -n=1
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                     PORTS     NAMES
f766035e7769   centos    "/bin/bash"   18 minutes ago   Exited (0) 3 minutes ago             beautiful_proskuriakova

# 显示容器的编号
$ docker ps -aq
f766035e7769
7e61c738bcf4
286484b695e4
```

退出容器

```powershell
$ exit    # 直接容器停止并退出
$ Ctrl + P + Q # 容器不停止推出
```

删除容器

```shell
$ docker rm 容器id                   # 删除制定的容器，不能删除正在运行的容器，如果要强制删除需要使用 rm -f 
$ docker rm -f $(docker ps -aq)     # 删除所有容器
$ docker ps -a -q | xargs docker rm # 删除所有的容器
```

启动和停止容器的操作

```powershell
$ docker start 容器id       # 启动容器
$ docker restart  容器id    # 重启容器
$ docker stop 容器id        # 停止当前正在运行的的容器
$ docker kill 容器id        # 强制停止当前容器
```

常用的其他命令

```powershell
# 命令：docker run -d 镜像名 
$ docker run -d centos

# 问题docker ps ,发现centos停止了

# 常见的坑：docker 容器使用后台运行，就必须要有一个前台进程，docker发现没有应用就会自动停止
# apache ,容器启动后，发现自己没有提供服务，就会停止，就没有程序了。
```

查看日志,[docker logs](https://docs.docker.com/engine/reference/commandline/logs/)

```powershell
$  docker logs -tf --tail 10 容器id 没有日志
# 如果docker镜像启动本来就没有日志，可以编写一个脚本进行测试

$ docker run -d centos /bin/sh -c "while true;do echo hello docker;sleep 1;done"
8095f554dbd16c29ef61182a12813756458a0a24d09bdbe761142dac1c971e7c

# 查看运行docker
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                                       NAMES
be72714b0e60   centos            "/bin/sh -c 'while t…"   16 seconds ago   Up 15 seconds                                               jovial_zhukovsky

# 显示日志
-tf        # 显示时间和日志
--tail num # 显示日志条数 

# 显示10行的be72714b0e60的日志
$ docker logs -tf --tail 10 be72714b0e60
2022-06-10T12:52:09.983041830Z hello docker
2022-06-10T12:52:10.984397462Z hello docker
2022-06-10T12:52:11.985690607Z hello docker
2022-06-10T12:52:12.986983999Z hello docker
2022-06-10T12:52:13.988244570Z hello docker
2022-06-10T12:52:14.989404639Z hello docker
2022-06-10T12:52:15.990945974Z hello docker
2022-06-10T12:52:16.992115773Z hello docker
2022-06-10T12:52:17.993503637Z hello docker
2022-06-10T12:52:18.995010360Z hello docker
```

查看容器进程信息

```powershell
$ docker top 容器id  # 查看指定容器的进程信息 
$ $ docker top be72714b0e60
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                650475              650452              0                   20:50               ?                   00:00:00            /bin/sh -c while true;do echo hello docker;sleep 1;done
root                682489              650475              0                   21:17               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
```

查看镜像的元数据

```powershell
# 命令
$ docker inspect 容器id   

# 查看centos 容器的元数据
$ docker inspect be72714b0e60
[
    {
        "Id": "be72714b0e60f4911bc807bbb4597e139f6eaa829c9ac117b76e2f13b87c1bf2",
        "Created": "2022-06-10T12:50:51.582969979Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo hello docker;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 650475,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-06-10T12:50:51.87753439Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/be72714b0e60f4911bc807bbb4597e139f6eaa829c9ac117b76e2f13b87c1bf2/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/be72714b0e60f4911bc807bbb4597e139f6eaa829c9ac117b76e2f13b87c1bf2/hostname",
        "HostsPath": "/var/lib/docker/containers/be72714b0e60f4911bc807bbb4597e139f6eaa829c9ac117b76e2f13b87c1bf2/hosts",
        "LogPath": "/var/lib/docker/containers/be72714b0e60f4911bc807bbb4597e139f6eaa829c9ac117b76e2f13b87c1bf2/be72714b0e60f4911bc807bbb4597e139f6eaa829c9ac117b76e2f13b87c1bf2-json.log",
        "Name": "/jovial_zhukovsky",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/0fc9cdacc3b9f7bb1a6445173c604dafba45749458ba4c8500dad1a8aac09b3a-init/diff:/var/lib/docker/overlay2/643b7419a5e24b28b4b201c15accae5d5b1a0caf84229d9cff1d8c068f18ac8e/diff",
                "MergedDir": "/var/lib/docker/overlay2/0fc9cdacc3b9f7bb1a6445173c604dafba45749458ba4c8500dad1a8aac09b3a/merged",
                "UpperDir": "/var/lib/docker/overlay2/0fc9cdacc3b9f7bb1a6445173c604dafba45749458ba4c8500dad1a8aac09b3a/diff",
                "WorkDir": "/var/lib/docker/overlay2/0fc9cdacc3b9f7bb1a6445173c604dafba45749458ba4c8500dad1a8aac09b3a/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "be72714b0e60",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo hello docker;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "2e1d253296845eb7e8e5dc418d9a2b9fa7a39b38e470f632270f3bb3612db632",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/2e1d25329684",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "98e8d239576f58070b4898d4ef16dc89a9afdb7f07d96f9980b52efeb028f2af",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.6",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:06",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "d64c7bbc42f591f3d4e7ce51fdbf9a93ac1bcf29459995c66db327dfce79003b",
                    "EndpointID": "98e8d239576f58070b4898d4ef16dc89a9afdb7f07d96f9980b52efeb028f2af",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.6",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:06",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

进入当前正在运行的容器

```powershell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令
$ docker exec -it 容器id bashshell

# 测试
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED             STATUS             PORTS                                       NAMES
be72714b0e60   centos            "/bin/sh -c 'while t…"   51 minutes ago      Up 51 minutes                                                  jovial_zhukovsky
lemu-devops@lemudevops:~$ docker exec -it be72714b0e60 /bin/bash
[root@be72714b0e60 /]# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 12:50 ?        00:00:00 /bin/sh -c while true;do echo hello docker;sleep 1;done
root        3133       0  0 13:43 pts/0    00:00:00 /bin/bash
root        3153       1  0 13:43 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
root        3154    3133  0 13:43 pts/0    00:00:00 ps -ef

# 方式二
$ docker attach 容器id 

# 测试
$ docker attach be72714b0e60
正在执行当前的代码..

# docker exec      # 进入容器开始一个新的终端，可以在里面操作（常用）
# docker attach    # 进入容器正在执行的终端，不会启动新的进程！
```

从容器拷贝文件到主机上

```shell
$ docker cp 容器id:容器内路径 目的主机路径

# 测试
# 查看当前主机目录的文件
root@lemudevops:/home/lemu-devops/docker_test# ls
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                                       NAMES
24f8d1099201   centos            "/bin/bash"              5 seconds ago   Up 4 seconds  
```$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                                       NAMES
24f8d1099201   centos            "/bin/bash"              5 seconds ago   Up 4 seconds  
35                   [OK]

# 进入容器内部
$ docker exec -it 24f8d1099201 /bin/bash
[root@24f8d1099201 /]# cd /home/
# 在容器内部创建一个文件
[root@24f8d1099201 home]# touch kizai.py
[root@24f8d1099201 home]# exit
exit

# 将文件拷贝到主机上
docker cp 24f8d1099201:/home/kizai.py /home/lemu-devops/docker_test
root@lemudevops:/home/lemu-devops/docker_test# ls
kizai.py

# 后面使用数据卷的技术才能把数据打通
```

#### docker命令小结

![](https://s2.loli.net/2022/06/11/Y8KOJcbwRX67mHB.png)

| 命令    | 解释                                                               | 中文解释                                                                                 |
| ------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| attach  | Attach to a running container                                      | # 当前she11下attach连接指定运行镜像                                                      |
| build   | Build an image from a Dockerfile                                   | # 通过Dockerfile定制镜像                                                                 |
| commit  | Create a new image from a container changes                        | # 提交当前容器为新的镜像                                                                 |
| cp      | Copy files/folders from the containers filesystem to the host path | # 从容器中拷贝指定文件或者目 录到宿主机中                                                |
| create  | Create a new container                                             | # 创建一个新的容器，同run,但不启动容器                                                   |
| diff    | Inspect changes on a container's filesystem                        | # 查看docker容器变化                                                                     |
| events  | Get real time events from the server                               | # 从docker服务获取容器实时事件                                                           |
| exec    | Run a command in.an existing container                             | # 在已存在的容器上运行命令                                                               |
| export  | Stream the contents of a container as a tar archive                | # 导出容器的内容流作为一个tar归档文件[对应                                               |
| import  | history Show the history of an image                               | # 展示一个镜像形成历史                                                                   |
| images  | List images                                                        | # 列出系统当前镜像                                                                       |
| import  | Create a new filesystem image from the contents of a tarbal1       | # 从tar包中的内容创建一个新的文件系统映像[对应export]                                    |
| info    | Display system-wide information                                    | # 显示系统相关信息                                                                       |
| inspect | Return low-level information on a container                        | # 查看容器详细信息                                                                       |
| kill    | Kill a running container                                           | # kill指定docker容器                                                                     |
| load    | Load an image from a tar archive                                   | # 从一个tar包中加载一个镜像[对应save]                                                    |
| login   | Register or Login to the docker registry server                    | # 注册或者登陆一个docker源服务器                                                         |
| logout  | Log out from a Docker registry server                              | # 从当前Docker registry退出                                                              |
| logs    | Fetch the logs of a container                                      | # 输出当前容器日志信息                                                                   |
| port    | Lookup the public-facing port which is NAT-ed to PRIVATE_PORT      | # 查看映射端口对应的容器内部源端                                                         |
| pause   | Pause all processes within a container                             | # 暂停容器                                                                               |
| ps      | List containers                                                    | # 列出容器列表                                                                           |
| pull    | Pull an image or a repository from the docker registry server      | # 从docker镜像源服务器拉取指定镜像或者库镜像                                             |
| push    | Push an image or a repository to the docker registry server        | # 推送指定镜像或者库镜像至docker源服务器                                                 |
| restart | Restart a running container                                        | # 重启运行的容器                                                                         |
| rm      | Remove one or more containers                                      | # 移除一个或者多个容器                                                                   |
| rmi     | Remove one or more images                                          | # 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除] |
| run     | Run a command in a.new container                                   | # 创建一个新的容器并运行一个命令                                                         |
| save    | Save an image to a tar archive                                     | # 保存一个镜像为一个tar包[对应load]                                                      |
| search  | Search for an image on the Docker Hub                              | # 在docker hub中搜索镜像                                                                 |
| start   | Start a stopped containers                                         | # 启动容器                                                                               |
| stop    | Stop a running containers                                          | # 停止容器                                                                               |
| tag     | Tag an image into a repository                                     | # 拾源中镜像打标签                                                                       |
| top     | Lookup the running processes of a container                        | # 查看容器中运行的进程信息                                                               |
| unpause | Unpause a paused container                                         | # 取消暂停容器                                                                           |
| version | Show the docker version information                                | # 查看docker版本号                                                                       |
| wait    | Block until a container stops,then print its exit code             | # 截取容器停止时的退出状态值                                                             |


## Docker镜像讲解
**UnionFS(联合文件系统)** 
* UnionFS(联合文件系统)：Union文件系统(UnionFS)是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改 作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。**Union文件系统是Docker镜像的基础。**镜像可以通过分层来进行继承，基于基础镜像（设有父镜像），可以制作各 种具体的应用镜像。
* 特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的 文件系统会包含所有底层的文件和目录


**Dockerk镜像加载原理**
* docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。
* bootfs(boot file system)主要包含bootloader和kernel,bootloader主要是引导加载kernel,,Linuxl刚启动时会加载bootfs文件系 统，在Docker镜像的最底层是oootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成 之后整个内核就都在内存中了，此时内存的使用权已由oootfs转交给内核，此时系统也会卸载bootfs。 
* rootfs(root file system),在bootfs之上。包含的就是典型Linux系统中的/dev,/proc,/bin,/etc等标准目录和文件。rootfs就是 各种不同的操作系统发行版，比如Ubuntu,Centos等等。

![](https://s2.loli.net/2022/06/11/tqNU2lyVdcCI4L1.png)

平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？

![](https://s2.loli.net/2022/06/11/kSKw3VzpU5JAZFr.png)

* 对于个精简的OS,rootfs可以很小，**只需要包合最基本的命令**，工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的Linux发行版， boots基本是一致的， rootfs会有差別，因此不同的发行版可以公用bootfs.
* 虚拟机是分钟级别，容器是秒级！

### 分层理解
下载redis镜像看看

![](https://s2.loli.net/2022/06/11/oRSKvU7B6PfIr51.png)

* 思考：为什么Docker镜像要采用这种分层的结构呢？
* 最大的好处是资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。
* 查看镜像分层的方式可以通过`docker image inspect` 命令

```shell
$ docker image inspect redis:latest
.....
 "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:ad6562704f3759fb50f0d3de5f80a38f65a85e709b77fd24491253990f30b6be",
                "sha256:49cba0f0997b2bb3a24bcfe71c7cbd6e9f6968ef7934e3ad56b0f1f9361b6b91",
                "sha256:309498e524b3e2da1f036d00cd5155e0b74cf9e1d964a3636c8ed63ca4a00d43",
                "sha256:86efad1273f723462d374c3c3191637ae6598e715312f8f1ec5929c3b70aee35",
                "sha256:9a2bd1816e9a1abda14f1acf07ca32ce4d43c52127836704a36d8fd1a1198ff2",
                "sha256:a7b30871a925a85530ee4bf40b5b875cc7ab0b452d9d571eb841cf92156083d8"
            ]
        },
.....
```
理解：

* 所有的 Docker镜像都起始于一个基础镜像层，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。
* 举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，
* 就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![](https://s2.loli.net/2022/06/11/gYaLhMsRweqitHB.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![](https://s2.loli.net/2022/06/11/3hGAy4QpJ2joBqX.png)

* 上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件
* 下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版

![](https://s2.loli.net/2022/06/11/dZoM2nzI768kTVv.png)

* 这种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中
* Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统
* Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的
件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。
* Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。
* 下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图

![](https://s2.loli.net/2022/06/11/cHysbCXA57vqlRr.png)

* 特点
  * Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！
  * 这一层就是我们通常说的容器层，容器之下的都叫镜像层！

![](https://s2.loli.net/2022/06/11/abmtjKfYWshe4lv.png)

* 总结：下载不同软件镜像时，分层的资源是可共用的！

### 如何提交一个自己的镜像
**commit 镜像**

```shell
$ docker commit 提交容器成为一个新的副本

# 命令跟git原理类似
$ docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```

实战测试

```shell
$ docker images
REPOSITORY        TAG       IMAGE ID       CREATED        SIZE
new_dingbot_msg   latest    cde233707f86   2 days ago     437MB
dingmsg_service   latest    269caf0cc7eb   3 days ago     437MB
redis             latest    dd8125f93b94   3 days ago     117MB
ubuntu            20.04     20fffa419e3a   5 days ago     72.8MB
mysql             latest    65b636d5542b   2 weeks ago    524MB
httpd             latest    98f93cd0ec3b   2 weeks ago    144MB
hello-world       latest    feb5d9fea6a5   8 months ago   13.3kB
centos            latest    5d0da3dc9764   8 months ago   231MB
# 启动httpd服务
$ docker run -it -p 8088:80 --name apache-tt -d httpd
d0f5368beceec8596197b734ff6be012b26eee32b3dae20df2a07ef17525e9a1
# 查看启动状态
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS             PORTS                                       NAMES
d0f5368becee   httpd             "httpd-foreground"       33 seconds ago   Up 31 seconds      0.0.0.0:8088->80/tcp, :::8088->80/tcp       apache-tt

$ docker exec -it d0f5368becee /bin/bash
root@d0f5368becee:/usr/local/apache2# ls
bin  build  cgi-bin  conf  error  htdocs  icons  include  logs  modules

root@d0f5368becee:/usr/local/apache2# cd htdocs/
root@d0f5368becee:/usr/local/apache2/htdocs# ls
index.html
root@d0f5368becee:/usr/local/apache2/htdocs# cat index.html 
<html><body><h1>It works!</h1></body></html>
# 注意httpd默认是没有安装vim的，需要更新一下源再安装
root@d0f5368becee:/usr/local/apache2/htdocs# apt-get update
....
root@d0f5368becee:/usr/local/apache2/htdocs# apt-get install vim
....
# 修改index.html的内容
root@d0f5368becee:/usr/local/apache2/htdocs# vim index.html
<html><body><h1>httpd run in docker!It works!</h1></body></html>
# 保存退出docker终端

# 查看修改httpd后的内容
$ wget 127.0.0.1:8088
--2022-06-12 18:07:58--  http://127.0.0.1:8088/
Connecting to 127.0.0.1:8088... connected.
HTTP request sent, awaiting response... 200 OK
Length: 65 [text/html]
Saving to: ‘index.html’

index.html                                                                 100%[=======================================================================================================================================================================================>]      65  --.-KB/s    in 0s      

2022-06-12 18:07:58 (17.0 MB/s) - ‘index.html’ saved [65/65]

$ cat index.html 
<html><body><h1>httpd run in docker!It works!</h1></body></html>

# 提交修改后的镜像
$ docker commit -a="kizai" -m="change the content of a index.html" d0f5368becee httpd-test:0.0.1
sha256:5ef56fce7bbbf2ad631214cc6e78ae497ce466b8fac50ec1b0b1550535f96f5e
$ docker images
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
httpd-test        0.0.1     5ef56fce7bbb   25 seconds ago   199MB

```

![](https://s2.loli.net/2022/06/12/C7wn59DvUPYzAgZ.png)

如果想要保存当前容器的状态，就可以通过commit来提交获得一个镜像。

![](https://s2.loli.net/2022/06/12/XGO4jyH1DtI3Ura.png)


### docker操作实例
1. 拉取官方的镜像

```powershell
$ docker pull httpd
```

2. 等待下载完成后，我们就可以在本地镜像列表里查到REPOSITORY为httpd的镜像。

```powershell
$ docker images httpd
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
httpd        latest    98f93cd0ec3b   11 days ago   144MB
```

#### 使用apache镜像
1. 运行容器

```powershell
$ docker run -it -p 8877:80 --name apache-test -d httpd 
986f882220ef2e78874d20e6ea7e6dbd2d62877e2026b1e2bbc489482e3291ae
```

* --name 为容器取一个名字
* -p 参数语法为 -p host port:container port; -p 8877:80 将主机上的8080端口绑定到容器上的80端口，因此在主机中访问8080端口时其实就是访问 nginx 容器的80端口
* -d 后台运行容器
2. 查看Docker运行情况

```powershell
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                        PORTS                                   NAMES
986f882220ef   httpd          "httpd-foreground"       33 seconds ago   Up 32 seconds                 0.0.0.0:8877->80/tcp, :::8877->80/tcp   apache-test
```

3. 查看容器运行日志

```shell
docker logs apache-test
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
[Wed Jun 08 15:09:22.925340 2022] [mpm_event:notice] [pid 1:tid 139932608978240] AH00489: Apache/2.4.53 (Unix) configured -- resuming normal operations
[Wed Jun 08 15:09:22.925448 2022] [core:notice] [pid 1:tid 139932608978240] AH00094: Command line: 'httpd -D FOREGROUND'
172.17.0.1 - - [08/Jun/2022:15:10:33 +0000] "GET / HTTP/1.1" 200 45
172.17.0.1 - - [08/Jun/2022:15:10:34 +0000] "GET /favicon.ico HTTP/1.1" 404 196
```
4. 打开浏览器输入`127.0.0.0:8877`就可以看到docker的httpd服务了。

### 使用Dockerfile文件来创建docker镜像

* Dockerfile 中的内置命令及其作用

| 内置命令   | 作用                                                              |
| ---------- | ----------------------------------------------------------------- |
| FROM       | 指明基础镜像的名称和版本。必填项                                  |
| MAINTAINER | 可用于提供作者、版本及其其他信息。可选项                          |
| RUN        | 用于执行后面的指令。当RUN执行完毕后，将产生一个新的文件层。可选项 |
| CMD        | 指定此镜像启动默认执行的命令。可选项                              |
| LABEL      | 用于在镜像中添加元数据。例如版本号、构建日期等。可选项            |
| EXPOSE     | 用于指定需要暴露的网络端口号。可选项                              |
| ENV        | 用于在镜像添加环境变量。可选项                                    |
| ADD        | 向镜像添加新文件或者新目录。可选项                                |
| COPY       | 从主句向镜像复制文件。可选项                                      |
| ENTRYPOINT | 在镜像中设定默认执行的二进制程序。可选项                          |
| VOLUME     | 向镜像中挂载一个卷组。可选项                                      |
| USER       | 在镜像构建过程中，生成或者切换到另外一个用户。可选项              |
| WORKDIR    | 设定此镜像后续操作默认的工作目录。可选项                          |
| ONBUILD    | 配置构建触发指令集。可选项                                        |

#### 创建钉钉机器人查询服务的API服务镜像

1. 创建文件构建镜像的文件夹,拷贝API服务代码到app目录下。

```powershell
$ cd ～
$ mkdir -p Docker_Project/Query_DingBot/app

# 此时的文件结构如下：
.
├── app
│   ├── config.ini
│   └── lemumsg_run.py
└── Dockerfile

1 directory, 3 files
```

2. 在`Query_DingBot`创建Dockerfile文件

```powershell
$ vim Dockerfile
# 添加以下内容
# 基于哪个镜像
FROM ubuntu:20.04

# 维护者信息
LABEL maintainer="kizai@foxmmail.com"
# 设置工作空间，后续命令会在此目录下执行
WORKDIR /app
# 添加文件到容器中
ADD . /app/
# 安装运行环境
RUN apt-get update && apt-get install -y python3 && apt-get install -y python3-pip && pip install DingtalkChatbot==1.5.3 && pip install flask==2.1.2 && pip install flask_json==0.3.4

# 开放6655端口
EXPOSE 6655

# 配置容器启动后执行的命令
ENTRYPOINT ["python3"]

CMD ["app/lemumsg_run.py"]

```
* `FROM`指令指定初始镜像。
* 所有`Dockerfile`一定要有`FROM`指令作为第一个非注释指令。
* RUN 指令指定的`shell`命令，是将要在镜像里执行的。

3. 在同一目录下执行docker build 命令：

```powershell
$ docker build -t dingmsg_service .
```

4. 查看构建成功的镜像：

```powershell
$ docker image ls
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
dingmsg_service   latest    cd4570fa31e6   2 minutes ago    437MB
```
5. 运行`dingmsg_service `镜像

```powershell
$ docker run -it --restart=always -p 6655:6655 --name dingbot-test -d dingmsg_service

# Docker 容器的重启策略如下：
 --restart具体参数值详细信息：
       no　　　　　　　 // 默认策略,容器退出时不重启容器；
       on-failure　　  // 在容器非正常退出时（退出状态非0）才重新启动容器；
       on-failure:3    // 在容器非正常退出时重启容器，最多重启3次；
       always　　　　  // 无论退出状态是如何，都重启容器；
       unless-stopped  // 在容器退出时总是重启容器，但是不考虑在 Docker 守护进程启动时就已经停止了的容器
```

6. 查看Docker运行的log

```log
$ docker logs dingbot-test 
 * Serving Flask app 'lemumsg_run' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on all addresses (0.0.0.0)
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://127.0.0.1:6655
 * Running on http://172.17.0.2:6655 (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 915-478-794
10.0.0.1 - - [09/Jun/2022 09:35:07] "POST /api/v1/text HTTP/1.0" 200 -
INFO:werkzeug:10.0.0.1 - - [09/Jun/2022 09:35:07] "POST /api/v1/text HTTP/1.0" 200 -
10.0.0.1 - - [09/Jun/2022 09:35:28] "POST /api/v1/text HTTP/1.0" 200 -
INFO:werkzeug:10.0.0.1 - - [09/Jun/2022 09:35:28] "POST /api/v1/text HTTP/1.0" 200 -
```
Docker的日志一般保存在`/var/lib/docker/. `每个容器都有一个特定于其 ID 的日志（完整 ID，而不是通常显示的缩短的 ID），您可以像这样访问它：
```powershell
/var/lib/docker/containers/ID/ID-json.log
```

7. 重启电脑后查看docker进程，dingbot-test进程服务依旧运行着。
8. 进入dingbot-test终端进行交互：

```powershell
$ docker exec -i -t dingbot-test /bin/bash
root@7e61c738bcf4:/app# ls
Dockerfile  app
root@7e61c738bcf4:/app# cd app/
root@7e61c738bcf4:/app/app# ls
config.ini  lemumsg_run.py
```
进到dingbot-test进程服务终端，其实可以发现里面的文件架构跟创建镜像时的是一样的。

### 导出/载入Docker镜像，以dingmsg_service镜像为例
#### 使用 export 和 import
1. 查看本机的容器。

```powershell
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED        STATUS             PORTS                                       NAMES
7e61c738bcf4   dingmsg_service   "python3 app/lemumsg…"   7 hours ago    Up 6 hours         0.0.0.0:6655->6655/tcp, :::6655->6655/tcp   dingbot-test
986f882220ef   httpd             "httpd-foreground"       25 hours ago   Up About an hour   0.0.0.0:8877->80/tcp, :::8877->80/tcp       apache-test
```

2. 使用 docker export 命令根据容器 ID 将镜像导出成一个文件。

```powershell
# 创建文件夹来保存镜像文件
$ mkdir -p ~/Docker_Project/images
# 导出镜像
$ docker export 7e61c738bcf4 > ~/Docker_Project/images/dingbot_msg_01.tar
# 导出成功不会有提示，可以查看该文件夹是否tar的镜像包
$ ls
dingbot_msg_01.tar
```

上面命令执行后，可以看到文件已经保存到当前的`~/Docker_Project/images`终端目录下。
3. 使用 `docker import` 命令则可将这个镜像文件导入进来。

```powershell
$ docker import - new_dingbot_msg < dingbot_msg_01.tar
sha256:cde233707f864c1578cb521d4ba88eee38f185b6435cc3478e49ec8e7aa07705
```

4. 执行 `docker images ls` 命令可以看到镜像确实已经导入进来了。

```shell
$ docker image ls
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
new_dingbot_msg   latest    cde233707f86   59 seconds ago   437MB
dingmsg_service   latest    269caf0cc7eb   7 hours ago      437MB
```
#### 使用 save 和 load
1. 查看本机的容器。

```powershell
$ docker image ls
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
new_dingbot_msg   latest    cde233707f86   15 minutes ago   437MB
dingmsg_service   latest    269caf0cc7eb   7 hours ago      437MB
ubuntu            20.04     20fffa419e3a   2 days ago       72.8MB
httpd             latest    98f93cd0ec3b   12 days ago      144MB
hello-world       latest    feb5d9fea6a5   8 months ago     13.3kB
```

2. 下面使用 `docker save` 命令根据 ID 将镜像保存成一个文件。

```powershell
$ docker save 269caf0cc7eb > dingbot_msg_02.tar 
```

3. 载入镜像,使用 `docker load` 命令则可将这个镜像文件载入进来。

```powershell
docker load < dingbot_msg_02.tar
```

>> 特别注意：两种方法不可混用。
   * 如果使用 `import` 导入 `save` 产生的文件，虽然导入不提示错误，但是启动容器时会提示失败，会出现类似`docker: Error response from daemon: Container command not found or does not exist`的错误。

#### 两种方案的差别
1. 文件大小不同:export 导出的镜像文件体积小于 save 保存的镜像
2. 是否可以对镜像重命名
   * `docker import` 可以为镜像指定新名称
   * `docker load` 不能对载入的镜像重命名
3. 是否可以同时将多个镜像打包到一个文件中
   * `docker export` 不支持
   * `docker save` 支持
4. 是否包含镜像历史
   * export 导出（import 导入）是根据容器拿到的镜像，再导入时会丢失镜像所有的历史记录和元数据信息（即仅保存容器当时的快照状态），所以无法进行回滚操作。
   * 而 save 保存（load 加载）的镜像，没有丢失镜像的历史，可以回滚到之前的层（layer）。
5. 应用场景不同
   * docker export 的应用场景：主要用来制作基础镜像，比如我们从一个 ubuntu 镜像启动一个容器，然后安装一些软件和进行一些设置后，使用 docker export 保存为一个基础镜像。然后，把这个镜像分发给其他人使用，比如作为基础的开发环境。
   * docker save 的应用场景：如果我们的应用是使用 `docker-compose.yml` 编排的多个镜像组合，但我们要部署的客户服务器并不能连外网。这时就可以使用 docker save 将用到的镜像打个包，然后拷贝到客户服务器上使用 docker load 载入。

## Docker数据卷
### 什么是数据卷
* 数据卷是一个可供一个或多个容器使用的特殊目录，它绕过`UFS`(联合文件系统[Union File System]，把其他文件系统联合到一个联合挂载点的文件系统服务)，可以提供很多有用的特性：
  * 数据卷可以在容器之间共享和重用
  * 对数据卷的修改会立马生效
  * 对数据卷的更新，不会影响镜像
  * 卷会一直存在，直到没有容器使用
*数据卷的使用，类似于 Linux 下对目录或文件进行 mount。

* docker的理念回顾
    * 将应用和环境打包成一个镜像！ 
* 数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！**需求：数据可以持久化** 
* MySQL,容器删了，删库跑路！**需求：MySQL数据可以存储在本地**！ 
* 容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！ 
* 这就是卷技术！目录的挂载，将我们容器内的目录，挂载到Liux上面！

![](https://s2.loli.net/2022/06/12/NeozDPEFuYaW2sU.png)

* 总结：容器的持久化和同步操作！容器见是可以数据共享的！

### 使用数据卷
#### 方式一：使用命令来挂载 -v
```shell
$ docker run -it -v 主机目录:容器目录 -p

# 在本机创建一个映射文件夹
$ mkdir -p /home/lemu-devops/Docker_Project/share

# 运行并挂载
$ docker run -it -v /home/lemu-devops/Docker_Project/share/centos:/home centos /bin/bash
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED              STATUS              PORTS                                       NAMES
b7d2b3ee3d32   centos            "/bin/bash"              About a minute ago   Up About a minute                                               thirsty_napier
# 启动起来之后可以通过docker inspect查看容器的数据
$ docker inspect b7d2b3ee3d32
...
"Mounts": [
            {
                "Type": "bind",
                "Source": "/home/lemu-devops/Docker_Project/share/centos",
                "Destination": "/home",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
...

```

![](https://s2.loli.net/2022/06/12/FCr1le7SVOIGpg8.png)

* 测试文件的同步

![](https://s2.loli.net/2022/06/12/O9YsnkNLpfGD4TI.png)

* 再来测试
  1. 停止容器
  2. 在宿主机上修改wenj
  3. 启动容器
  4. 容器内的数据依旧是同步的。

![](https://s2.loli.net/2022/06/12/1gthMQw3YodbuOm.png)

好处：以后修改只需要在本都修改即可，容器内会自动同步。

### 实战：安装MySQL
思考：数据持久化的问题 
```powershell
# 获取镜像
$ docker pull mysql:5.7

# 运行容器，需要做数据挂载! 安装启动mysql，需要配置密码
# 官方测试配置：$ docker run --name some-mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
# 启动mysql
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
$ docker run -d -p 3310:3306 -v /home/lemu-devops/Docker_Project/share/mysql/conf:/etc/mysql/conf.d -v /home/lemu-devops/Docker_Project/share/mysql/data:/var/lib/mysql  -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

# 启动成功之后使用，mysql插件来测试一下
# mysql插件链接到服务器的3310 --- 3310和容器内的3306映射，这个时候就可以连上了

```

![](https://s2.loli.net/2022/06/12/wvJN6fzp7SFEo8H.png)

挂载文件成功的实例

![](https://s2.loli.net/2022/06/12/zmcGweRKso48pCh.png)

删除mysql容器查看主机的文件是否会删除呢？

![](https://s2.loli.net/2022/06/12/Sslmh2ugzYqQoJO.png) 

* 发现，我们挂载到本地的数据依旧没有丢失，这就实现了容器数据持久化的功能。

#### 具名和匿名挂载

```shell
# 匿名挂载
-v 容器内路径
$ docker run -d -P --name httpd02 -v /usr/local/apache2 httpd

# 查看所有volume 的情况
$ docker volume ls
DRIVER    VOLUME NAME
local     3baac0bfac36eca270cb3935e83f95db4140e3a2c85bf00d848ec12817b0f7c4 (匿名卷)
local     79de0d956d465f57a3731395041ff2c566d15c2b8a837c2f992c1d15f1cdaef0 (匿名卷)

# 这里发现,这种就是匿名挂载，我们在-v 只写了容器内的路径，没有写容器外的路径

# 具名挂载
$ docker run -d -P --name htppd03 -v juming-apache:/usr/local/apache2 httpd
b484b2f53946b46ce3320ef070af0fa9f935e2da5c13f571a46e58a35752d603
$ docker volume ls
DRIVER    VOLUME NAME
local     3baac0bfac36eca270cb3935e83f95db4140e3a2c85bf00d848ec12817b0f7c4
local     79de0d956d465f57a3731395041ff2c566d15c2b8a837c2f992c1d15f1cdaef0
local     juming-apache

# 通过 -v 卷名：容器内路径
# 查看一下这个卷
$ docker volume inspect juming-apache
[
    {
        "CreatedAt": "2022-06-12T23:00:39+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-apache/_data",
        "Name": "juming-apache",
        "Options": null,
        "Scope": "local"
    }
]

$ cd /var/lib/docker/
root@lemudevops:/var/lib/docker# ls
buildkit  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes
root@lemudevops:/var/lib/docker# cd volumes/
root@lemudevops:/var/lib/docker/volumes# ls
3baac0bfac36eca270cb3935e83f95db4140e3a2c85bf00d848ec12817b0f7c4  backingFsBlockDev  metadata.db
79de0d956d465f57a3731395041ff2c566d15c2b8a837c2f992c1d15f1cdaef0  juming-apache
root@lemudevops:/var/lib/docker/volumes# cd juming-apache/
root@lemudevops:/var/lib/docker/volumes/juming-apache# ls
_data
root@lemudevops:/var/lib/docker/volumes/juming-apache# cd _data/
root@lemudevops:/var/lib/docker/volumes/juming-apache/_data# ls
bin  build  cgi-bin  conf  error  htdocs  icons  include  logs  modules

```

所有docker容器的卷，没有指定目录的情况下都是在 `/var/lib/docker/volumes/xxx/_data` 路径下

我们通过具名挂载可以方便的找到我们的一个卷，大多数情况都是使用`具名挂载`。

```shell
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载
-v 容器内路径             # 匿名挂载
-v 卷名:容器内路径         # 具名挂载
-v /宿主机路径:/容器内路径  # 指定路径挂载
```

拓展

```shell
ro      readonly    # 只读
rw      readwrite   # 可读可写

# 一旦设置了容器权限，容器对我们挂载出来的内容就有限定了
$ docker run -d -P --name htppd04 -v juming-apache:/usr/local/apache2:ro httpd
$ docker run -d -P --name htppd04 -v juming-apache:/usr/local/apache2:rw httpd

# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部是无法操作的！！！

```

Dockerfile就是用来构建docker镜像的构建文件！命令脚本！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层。

```shell
# 创建一个dockerfile文件，名字可以随机，建议使用Dockerfile
# 文件内容指令(大写) 参数
$ mkdri -p /home/lemu-devops/Docker_Project/docker-test-volume
$ cd Docker_Project/docker-test-volume/
$ vim dockerfile1
# 写入dockerfile 命令，这里的每个命令都是镜像的一层
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "----end----"
CMD /bin/bash
# 保存退出
# 构建镜像
$ docker build -f dockerfile1 -t kizai/centos .

```

![](https://s2.loli.net/2022/06/13/BMbOXSYvV8juNdf.png)

### 数据卷容器
多个mysql同步数据

![](https://s2.loli.net/2022/06/13/nTyNdXYaVL9u7RD.png)

```shell
# 启动3个容器，通过自己写的镜像启动

```

![](https://s2.loli.net/2022/06/13/AULRn3jqYSD9crv.png)

启动自定义的容器

```shell
$ docker run -it b0248e10a05e /bin/bash
```

![](https://s2.loli.net/2022/06/13/o5hmJSwcr6K12F3.png)

这个卷的外部一定有一个同步的目录

![](https://s2.loli.net/2022/06/13/lmyktKOe92VG8pu.png)

查看卷挂载的路径

![](https://s2.loli.net/2022/06/13/KLU4eP1ME5Xyo7g.png)

测试一下文件是否同步过去了

![](https://s2.loli.net/2022/06/13/4TFAsOt39ZPuJQy.png)

假设构建镜像时没有挂载卷，要手动镜像挂载 `-v 卷名:容器内路径`

启动第docker02数据卷继承于docker01，在这里docker01就叫做数据卷容器。

```shell
$ docker run -it --name docker02 --volumes-from docker01 b0248e10a05e
```

docker01创建的数据同步到docker02上
  
![](https://s2.loli.net/2022/06/13/gu32LdpEONAb1I5.png)

启动docker03数据卷继承于docker01

![](https://s2.loli.net/2022/06/13/W2ewhcRMsf6EFJT.png)

测试：可以删除docker01,查看docker02和docker03是否可以访问这个文件？

经过测试：删除docker01后，docker02和docker03一样是可以访问文件的。

![](https://s2.loli.net/2022/06/13/QBz3wqySsZOthvF.png)

多个mysql实现数据共享

```shell
# 启动mysql01
$ docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
# 启动mysql02
$ docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01 mysql:5.7

# 这个时候就可以实现两个容器数据同步
```
### 结论
容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。

但是一旦持久化到本地，这个时候，本地是数据是不会删除的。

## DockerFile
dockerfile 是用来构建docker镜像的文件，命令参数脚本！

构建步骤：
    1. 编写一个dockerfile文件
    2. `docker build`构建成为一个镜像
    3. `docker run`运行镜像
    4. `docker push`发布到（DockerHub、私有DockerHub服务）

查看一下官方是怎么做到？

![](https://s2.loli.net/2022/06/13/V3UjsCebcq6ATRp.png)

![](https://s2.loli.net/2022/06/13/XdCjaBRVTzYh2w9.png)

很多官方的镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像。

### DockerFile构建过程
#### 基础知识：
1. 每个保留关键字（指令）都必须是大写字母
2. 执行从上到下顺序执行
3. `#`表示注释
4. 每一个指令都会创建提交一个新的镜像层并提交。

![](https://s2.loli.net/2022/06/13/2czSBeiVtgK4yOm.png)

dockerfile是面向开发的，以后要发布项目做镜像，就需要编写dockerfile文件，这个文件比较简单！

Docker镜像逐渐成为企业交互的标准！

### 步骤：开发、部署、运维。。。
1. Dockerfile:构建文件，定义了一切的步骤，源代码。
2. DockerImage:通过Dockerfile构建生成的镜像，最终发布和运行的产品
3. Docker容器：容器就是镜像运行起来提供服务器

### DockerFile的指令

```dockerfile
FROM            # 基础镜像
MAINTAINER      # 镜像谁写的，姓名+邮箱
RUN             # 镜像构建的时候需要运行的命令
ADD             # 步骤:centos镜像，centos压缩包！添加内容
WORKDIR         # 镜像的工作目录
VOLUME          # 挂载的目录
EXPOSE          # 暴露端口配置
CMD             # 指定这个容器启动的时候运行的命令，只有最后一个会生效，可以被替代
ENTRYPOINT      # 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD         # 当构建一个被继承 DockerFile 这个时候就会运行 ONBUILD 的指令。触发指令
COPY            # 类似ADD,将我们文件拷贝到镜像中
ENV             # 构建的时候设置环境变量
```

![](https://s2.loli.net/2022/06/13/Tkv9ntPbhSOFsER.png)

### 实战测试
Docker Hub中99%镜像都是从这个基础镜像过来的`FROM scratch`,然后配置需要的软件和配置来进行构建。

创建一个自己的centos

```dockerfile

# 1.编写dockerfile文件
$ vim dockerfile-centos
FROM centos:latest
MAINTAINER kizai<kizai@foxmail.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

EXPOSE 80

CMD echo $MYPATH
CMD echo "-----end-----"
CMD /bin/bash

# 2.通过这个文件构建镜像
# 命令: docker build -f dockerfile文件路径 -t 镜像名:[tag]
Successfully built 602aa38c9a1a
Successfully tagged mycentos:0.1

# 测试运行
```

![](https://s2.loli.net/2022/06/13/FkLwYxv3UVT6fPe.png)

### CMD 和 ENTRYOINT 的区别

```shell
CMD             # 指定这个容器启动的时候运行的命令，只有最后一个会生效，可以被替代
ENTRYPOINT      # 指定这个容器启动的时候要运行的命令，可以追加命令
```

测试CDM

```shell
# 编写dockerfile文件
$ vim dockerfile-cmd-test
FROM centos
CMD ["ls","-a"]

# 构建镜像
$ docker build -f dockerfile-cmd-test -t cmdtest .

# run运行,发现我们的ls -a命令生效了
$ docker run 933abee7baa1
.
..
.dockerenv
bin
dev
etc
home
lib
lib64

# 想追加一个命令-l ls -al
$ docker run 933abee7baa1 -l
docker: Error response from daemon: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "-l": executable file not found in $PATH: unknown.

# cmd的命令下 -l 替换了CMD ["ls","-a"] 命令，-l 不是命令所以报错！
```

测试ENTRYPOINT

```shell
$ vim dockerfile-cmd-entrypoint
FROM centos
CMD ["ls","-a"] 

# 构建镜像
$ docker build -f dockerfile-cmd-entrypoint -t entrypoint-test .
Sending build context to Docker daemon  4.608kB
Step 1/2 : FROM centos
 ---> 5d0da3dc9764
Step 2/2 : ENTRYPOINT ["ls","-a"]
 ---> Running in 6ee8bc2055a2
Removing intermediate container 6ee8bc2055a2
 ---> e2414dc3a0c9
Successfully built e2414dc3a0c9
Successfully tagged entrypoint-test:latest
# 查看镜像
$ docker images
REPOSITORY        TAG       IMAGE ID       CREATED             SIZE
entrypoint-test   latest    e2414dc3a0c9   9 seconds ago       231MB
cmdtest           latest    933abee7baa1   11 minutes ago      231MB
mycentos          0.1       0f0d9b408fbd   About an hour ago   231MB
kizai/centos      latest    b0248e10a05e   9 hours ago         231MB
httpd-test        0.0.1     5ef56fce7bbb   29 hours ago        199MB
new_dingbot_msg   latest    cde233707f86   3 days ago          437MB
dingmsg_service   latest    269caf0cc7eb   4 days ago          437MB
redis             latest    dd8125f93b94   4 days ago          117MB
ubuntu            20.04     20fffa419e3a   6 days ago          72.8MB
mysql             5.7       2a0961b7de03   2 weeks ago         462MB
mysql             latest    65b636d5542b   2 weeks ago         524MB
httpd             latest    98f93cd0ec3b   2 weeks ago         144MB
hello-world       latest    feb5d9fea6a5   8 months ago        13.3kB
centos            latest    5d0da3dc9764   9 months ago        231MB
# 运行镜像
$ docker run e2414dc3a0c9
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
# 追加命令运行镜像，是直接拼接我们的 ENTRYOINT 命令后面
$ docker run e2414dc3a0c9 -l
total 56
drwxr-xr-x   1 root root 4096 Jun 13 15:12 .
drwxr-xr-x   1 root root 4096 Jun 13 15:12 ..
-rwxr-xr-x   1 root root    0 Jun 13 15:12 .dockerenv
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root  340 Jun 13 15:12 dev
drwxr-xr-x   1 root root 4096 Jun 13 15:12 etc
drwxr-xr-x   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root 4096 Sep 15  2021 lost+found
drwxr-xr-x   2 root root 4096 Nov  3  2020 media
drwxr-xr-x   2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 643 root root    0 Jun 13 15:12 proc
dr-xr-x---   2 root root 4096 Sep 15  2021 root
drwxr-xr-x  11 root root 4096 Sep 15  2021 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 Jun 13 15:12 sys
drwxrwxrwt   7 root root 4096 Sep 15  2021 tmp
drwxr-xr-x  12 root root 4096 Sep 15  2021 usr
drwxr-xr-x  20 root root 4096 Sep 15  2021 var
```

Dockerfile中很多命令都十分相似，我们需要了解他们的区别，最后的方法就是做对比测试。

### 小结

![](https://s2.loli.net/2022/06/13/1gATCi5uIzPmdn2.png)

## Docker网络
### 理解Docker0
清空所有的容器和镜像

```shell
# 删除所有容器
$ docker rm -f $(docker ps -aq)

# 删除所有镜像
$ docker image rm -f $(docker image -aq)
```

![](https://s2.loli.net/2022/06/14/MO29NblDeQdjT5w.png)

* 回环地址：不属于任何一个有类别地址类。它代表设备的本地虚拟接口，所以默认被看作是永远不会宕掉的接口,一般都会用来检查本地网络协议、基本数据接口等是否正常的。
* lo：local的简写，一般指本地环回接口。
* docker0：Docker使用的是Linux的桥接，在宿主机中是一个Docker容器的网桥docker0。每启动一个docker容器，docker就会给docker容器分配一个ip，只要安装了docker，就会有一个网卡docker0。
* oray_vnc:蒲公英路由组网生成的oray_vnc网关地址
* lft代表终身。如果您通过 DHCP 获取此地址，则它指的是相对于 IP 地址的租用时间。
* enp1s0: 为什么是 enp1s0 而不是 eth0?
  * 新的命名方案被称为"可预测的网络接口Predictable Network Interface"。 它已经在基于systemd 的 Linux 系统上使用了一段时间了。 接口名称取决于硬件的物理位置。 `en` 仅仅就是 `ethernet` 的意思，就像`eth` 用于对应 `eth0`一样。 `p` 是以太网卡的总线编号，`s` 是插槽编号。 所以`enp1s0`告诉我们很多我们正在使用的硬件的信息。
  * 使用状态

```shell
  # BROADCAST   该接口支持广播
  # MULTICAST   该接口支持多播
  # UP          网络接口已启用
  # LOWER_UP    网络电缆已插入，设备已连接至网络
  # DOWN        网络接口已关闭
  # UNKNOWN     暂不知道网络接口的状态
```

其他命令解释：

```shell
mtu 1500                                    最大传输单位（数据包大小）为1,500字节
qdisc pfifo_fast                            用于数据包排队
state UP                                    网络接口已启用
group default                               接口组
qlen 1000                                   传输队列长度
link/ether d8:bb:c1:17:ae:68                接口的 MAC（硬件）地址
brd ff:ff:ff:ff:ff:ff                       广播地址
inet 10.0.0.131/23                          IPv4 地址
brd 10.0.1.255                              广播地址
scope global                                全局有效
dynamic enp1s0                              地址是动态分配的
valid_lft 75239sec                          IPv4 地址的有效使用期限
preferred_lft 75239sec                      IPv4 地址的首选生存期
inet6 fd5c:a4db:ced8:0:681:9546:4ec:c846/64 IPv6 地址
scope link                                  仅在此设备上有效
valid_lft forever                           IPv6 地址的有效使用期限
preferred_lft forever                       IPv6 地址的首选生存期
```

#### docker重启自动运行的原理？怎么让它自动运行？
原理：Docker 容器的自动重启是由 Docker 守护进程完成的。

```shell
# 运行命令：
$ docker run -i -t -d --name dockername --restart=always imageId

# --restart的参数
no - container          #不重启
on-failure - container  # 退出状态非0时重启
on-failure:n            # 在容器非正常退出时重启容器，并且指定重启次数。n 为正整数。如果不指定次数，则会一直重启。
always                  # 始终重启
unless-stopped          # 在容器退出时总是重启容器，但是 Docker 守护进程启动之前就已经停止运行的容器不算在内。
```

* 容器启动时忘记使用 `--restart=always` 如何补救
    1. 使用 `update` 命令
    ```shell
    $ docker container update --restart=always 容器id
    ```
    2. 修改容器的配置文件
    ```shell
    # vim /var/lib/docker/containers/容器ID/hostconfig.json，找到关键字 RestartPolicy，将 no 改为 always修改前：

    "RestartPolicy:{"Name":"no","MaximumRetryCount":0}

    # 修改后：

    "RestartPolicy:{"Name":"always","MaximumRetryCount":0}

    # 重启容器即可。如果无法修改容器的配置，可先将容器停止，修改配置文件后再启动。
    ```

#### 文件系统是怎么挂载的？
AUFS 是联合文件系统，意味着它在主机上使用多层目录存储，**每一个目录在 AUFS 中都叫作分支，而在 Docker 中则称之为层(layer)，但最终呈现给用户的则是一个普通单层的
文件系统，我们把多层以单一层的方式呈现出来的过程叫作联合挂载。**

![](https://s2.loli.net/2022/06/14/ndDE1ljzpkbumXH.png)

如上图，每一个镜像层和容器层都是 /var/lib/docker 下的一个子目录，镜像层和容器层都在 aufs/diff 目录下，每一层的目录名称是镜像或容器的 ID 值，联合挂载点在 aufs/mnt 目录下，mnt 目录是真正的容器工作目录。

下面我们针对 aufs 文件夹下的各目录结构，在创建容器前后的变化做详细讲述。

* 当一个镜像未生成容器时，AUFS 的存储结构如下。
  * diff 文件夹：存储镜像内容，每一层都存储在以镜像层 ID 命名的子文件夹中。
  * layers 文件夹：存储镜像层关系的元数据，在 diff 文件夹下的每个镜像层在这里都会有一个文件，文件的内容为该层镜像的父级镜像的 ID。
  * mnt 文件夹：联合挂载点目录，未生成容器时，该目录为空。
* 当一个镜像已经生成容器时，AUFS 存储结构会发生如下变化。
  * diff 文件夹：当容器运行时，会在 diff 目录下生成容器层。
  * layers 文件夹：增加容器层相关的元数据。
  * mnt 文件夹：容器的联合挂载点，这和容器中看到的文件内容一致。

#### 如何理解“UNIX里一切都是文件这句话”
* 在LINUX系统中有一个重要的概念：一切都是文件。 其实这是UNIX哲学的一个体现，而Linux是重写UNIX而来，所以这个概念也就传承了下来。在UNIX系统中，把一切资源都看作是文件，包括硬件设备。UNIX系统把每个硬件都看成是一个文件，通常称为设备文件，这样用户就可以用读写文件的方式实现对硬件的访问。这样带来优势也是显而易见的：
  * 实现了设备无关性。
  * UNIX 权限模型也是围绕文件的概念来建立的，所以对设备也就可以同样处理了。

* 这一关键设计原则**提供了一个统一的范式**，用于访问各种输入输出资源：文档、目录、磁盘驱动器、CD-ROM、调制解调器、键盘、打印机、显示器、终端，甚至是一些进程间通信和网络通信。所有这些资源拥有一个通用的抽象，UNIX 之父将其称为“文件”。因为每个“文件”都通过相同的 API 暴露出来，所以你可以使用同一组基本命令来读取和写入磁盘、键盘、文档或网络设备。

* Linux文件系统的层次

![](https://s2.loli.net/2022/06/15/OVbSAZIorFJNpQG.png)

* 由上图可以知道，整个文件系统体系分为了三个层面，用户层，内核层，硬件层，用户层是通过API通过系统调用调用的方式访问虚拟文件系统。在内核层，我们可以看到虚拟文件系统下连接了各种类型的文件系统，其是对不同的文件系统的抽象，为上层应用提供了统一的 API 接口；上图内核层还有一层是各个文件系统之下的一层，这一层的作用是隐藏了不同硬件设备之间的细节，为内核提供了统一的 IO 操作接口。下面我们对整个文件系统从下到上对各个层的作用进行一个阐述：
* Device Driver(硬盘驱动)：常见的硬盘类型有PATA,SATA，在Linux中，对于硬盘提供的驱动模块一般都存放在内核目录树drivers/ata中，而对于一般通用的硬盘驱动，可能会直接被编译到内核中。
* 通用块设备层(General Block Device Layer):不同的硬盘，会提供不同的 IO 接口，对于内核来讲，这种杂乱的接口是不利于管理的，因此就把这些接口进行了抽象，形成了一个统一的对外接口，这样就不管你是什么存储设备，操作他们的IO接口并没有什么区别。
* 文件系统层：目前大多数Linux使用的是ex4,与此同时，btrfs也呼之欲出
* 虚拟文件系统：正如不同的存储设备具有不同的 IO 接口，那么不同的文件系统也具有不用的 API，内核想实现的是不管是什么文件系统，都采用的是相同的 API 进行操作，所以 VFS 就做了一个抽象，提供了统一的 API 接口，使之可以对不同的文件系统采用同样的操作。

* Linux广义文件分类

![](https://s2.loli.net/2022/06/15/GNtgnUWRq8hPd72.png)


####  docker引擎的log是在什么位置？

```shell
$ journalctl -u docker.service
-- Logs begin at Fri 2022-04-22 15:14:03 CST, end at Wed 2022-06-15 00:34:32 CST. --
6月 08 16:43:54 lemudevops systemd[1]: Starting Docker Application Container Engine...
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.930812506+08:00" level=info msg="Starting up"
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.931480314+08:00" level=info msg="detected 127.0.0.53 nameserver, assuming >
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.955378572+08:00" level=info msg="parsed scheme: \"unix\"" module=grpc
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.955408951+08:00" level=info msg="scheme \"unix\" not registered, fallback >
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.955436012+08:00" level=info msg="ccResolverWrapper: sending update to cc: >
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.955443385+08:00" level=info msg="ClientConn switching balancer to \"pick_f>
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.957511027+08:00" level=info msg="parsed scheme: \"unix\"" module=grpc
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.957547454+08:00" level=info msg="scheme \"unix\" not registered, fallback >
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.957582165+08:00" level=info msg="ccResolverWrapper: sending update to cc: >
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.957601264+08:00" level=info msg="ClientConn switching balancer to \"pick_f>
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.972856430+08:00" level=warning msg="Your kernel does not support CPU realt>
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.972876409+08:00" level=warning msg="Your kernel does not support cgroup bl>
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.972883379+08:00" level=warning msg="Your kernel does not support cgroup bl>
6月 08 16:43:54 lemudevops dockerd[45327]: time="2022-06-08T16:43:54.973026127+08:00" level=info msg="Loading containers: start."
6月 08 16:43:55 lemudevops dockerd[45327]: time="2022-06-08T16:43:55.047876403+08:00" level=info msg="Default bridge (docker0) is assigned with>
6月 08 16:43:55 lemudevops dockerd[45327]: time="2022-06-08T16:43:55.082275669+08:00" level=info msg="Loading containers: done."
6月 08 16:43:55 lemudevops dockerd[45327]: time="2022-06-08T16:43:55.101418786+08:00" level=info msg="Docker daemon" commit=a89b842 graphdriver>
6月 08 16:43:55 lemudevops dockerd[45327]: time="2022-06-08T16:43:55.101494435+08:00" level=info msg="Daemon has completed initialization"
6月 08 16:43:55 lemudevops systemd[1]: Started Docker Application Container Engine.
6月 08 16:43:55 lemudevops dockerd[45327]: time="2022-06-08T16:43:55.121873917+08:00" level=info msg="API listen on /run/docker.sock"
6月 08 16:44:50 lemudevops dockerd[45327]: time="2022-06-08T16:44:50.229069704+08:00" level=info msg="ignoring event" container=08dead044938eb9>
6月 08 17:09:02 lemudevops systemd[1]: Stopping Docker Application Container Engine...
6月 08 17:09:02 lemudevops dockerd[45327]: time="2022-06-08T17:09:02.118351683+08:00" level=info msg="Processing signal 'terminated'"
6月 08 17:09:02 lemudevops dockerd[45327]: time="2022-06-08T17:09:02.119070504+08:00" level=info msg="stopping event stream following graceful >
6月 08 17:09:02 lemudevops dockerd[45327]: time="2022-06-08T17:09:02.119357146+08:00" level=info msg="Daemon shutdown complete"
```

三个网络

```shell
# 问题:docker 是如何处理处理容器网络访问的？
```

![](https://s2.loli.net/2022/06/14/giv6ulUTnpwqPzJ.png)

```shell
$ docker run -d -P --name centos-test centos

# 查看容器的内部网络地址： ip addr

```


## Docker Compose

## Docker Swarm

## CI/CD