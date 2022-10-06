# 搭建私有Docker Hubs镜像库

### 修订记录

| 更新日期   | 版本号 | 版本说明                                                                   |
| ---------- | ------ | -------------------------------------------------------------------------- |
| 2022-06-16 | 1.0.0  | 搭建无认证私有镜像仓库                                                     |
| 2022-06-20 | 1.1.0  | 创建服务器的自签证书和生成用户名和密码进行登录; Docker镜像仓库的管理和运维 |
| 2022-06-22 | 1.1.1  | 把Docker部署到Gitlab和配置好Gitlab-Runner                                  |

* [Deploy a registry server](https://docs.docker.com/registry/deploying/)

## 一、环境准备
1. 主机：
   * Ubuntu 20.04 
   * IP:10.0.0.131 
   * 用作普通用户机
2. 虚拟机：
   * Ubuntu 22.04 
   * ip:10.0.0.139
   * Port:5000
   * 用作私有仓库服务机

## 二、Docker Registry搭建
### 1. 在虚拟机安装Docker引擎

```shell
// 更新apt包索引并安装包以允许apt通过 HTTPS 使用存储库：
$ sudo apt-get update
$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

// 添加 Docker 的官方 GPG 密钥：
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

// 使用以下命令设置存储库：
$  echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 

// 安装 Docker 引擎
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

// 查看ubuntu22.04支持的版本
$ apt-cache madison docker-ce
 docker-ce | 5:20.10.17~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.14~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
 docker-ce | 5:20.10.13~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages

// 安装5:20.10.17~3-0~ubuntu-jammy
$ sudo apt-get install docker-ce=5:20.10.17~3-0~ubuntu-jammy docker-ce-cli=5:20.10.17~3-0~ubuntu-jammy containerd.io docker-compose-plugin

// 查看docker版本，确保跟用户机版本一致
$ docker -v
Docker version 20.10.17, build 100c701

```

### 2. 配置docker 注册表
* 方式1：不使用配置文件来配置注册表

```bashshell
// 拉取 registry 镜像，下面是使用registry:2.6.2版本的搭建过程
$ docker pull registry:2.6.2

// 查看 registry 镜像是否拉取成功
$ docker images | grep registry

// 启动 registry 并将仓库数据映射到本地指定目录。默认情况下，会将仓库放在容器内的/var/lib/registry/目录下，这样有个问题，如果容器被删除，则存放在容器中的镜像也会丢失。
$ docker run -d -p 5000:5000 -v /docker/registry/:/var/lib/registry/ --name registry --restart=always registry:2.6.2

// 查看容器启动情况
$ docker ps
75cd1af29707   registry   "/entrypoint.sh /etc…"   27 seconds ago   Up 26 seconds   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp  registry:2.6.2

```

* 方式2：使用配置文件来配置注册表

```bashshell
// 如果默认配置不是您使用的合理基础，或者如果您在从环境中覆盖密钥时遇到问题，您可以通过将其安装为容器中的卷来指定备用 YAML 配置文件。
// 通常，从头开始创建一个名为的新配置文件config.yml，然后在docker run命令中指定它：
$ vim config.yml
// 写入以下内容
version: 0.1
log:
  level: debug
storage:
    filesystem:
        rootdirectory: /var/lib/registry
http:
    addr: localhost:5000
    secret: asecretforlocaldevelopment
    debug:
        addr: localhost:5001

$ docker run -d -p 5000:5000 --restart=always --name registry \
            -v `pwd`/config.yml:/etc/docker/registry/config.yml \
            -v /docker/registry/:/var/lib/registry/ \
            registry:2.6.2

// 查看容器运行情况
$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                                       NAMES
49dcf33f7e92   registry:2.6.2   "/entrypoint.sh /etc…"   16 seconds ago   Up 15 seconds   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   registry

```

### 3. 在普通机上配置私有仓库地址(**服务机如果需要推送镜像也需要做以下的配置！**)
如果不配置私有仓库地址会报`http: server gave HTTP response to HTTPS client`；出现这问题的原因是：Docker自从1.3.X之后docker registry交互默认使用的是HTTPS，但是搭建私有镜像默认使用的是HTTP服务，所以与私有镜像推送时出现以上错误。

```bashshell
// 在 /etc/docker/daemon.json 新增配置文件中添加 "insecure-registries": [ "10.0.0.139:5000"] 此处IP为私有仓库IP
$ sudo vim /etc/docker/daemon.json 
{
    "registry-mirrors": [
        "https://eoxejquzx.mirror.aliyuncs.com",
        "https://registry.docker-cn.com"
        ],
    "insecure-registries": ["10.0.0.139:5000"]
}

// 修改完成后重启docker服务,如果重启服务失败，请检查/etc/docker/daemon.json的内容是否正确
$ sudo systemctl daemon-reload 
$ sudo systemctl restart docker.service

// 可以使用docker info查看配置是否应用成功
$ docker info
....
 Insecure Registries:
  10.0.0.139:5000
  127.0.0.0/8
 Registry Mirrors:
  https://eoxejquzx.mirror.aliyuncs.com/
  https://registry.docker-cn.com/
 Live Restore Enabled: false
....
```
### 4. 在普通用户机上提交本地的镜像到私有仓库
```bashshell
// 命令
$ docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
-a :提交的镜像作者；
-c :使用Dockerfile指令来创建镜像；
-m :提交时的说明文字；
-p :在commit时，将容器暂停。

// 查看用户机的全部容器
$ docker ps -a
CONTAINER ID   IMAGE            COMMAND                  CREATED        STATUS                    PORTS                                       NAMES
38016114e6b3   dingbotmsg:1.1   "python3 app/lemumsg…"   10 hours ago   Up 10 hours               0.0.0.0:6655->6655/tcp, :::6655->6655/tcp   dingbot-api

// 提交dingbot-api容器生成镜像
$ docker commit -a "kizai<kziai@foxmail.com>" -m "commit to docker hub" dingbot-api 10.0.0.139:5000/lemumsg-api:1.0

// 查看docker镜像
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
10.0.0.139:5000/lemumsg-api   1.0       d552ded0dac3   10 seconds ago   439MB

// 推送镜像到私有仓库
$ docker push 10.0.0.139:5000/lemumsg-api:1.0
The push refers to repository [10.0.0.139:5000/lemumsg-api]
f39deba227af: Pushed 
ca4513b2f931: Pushed 
78698be0aa58: Pushed 
36bfc519b3b5: Pushed 
ef8e088d604d: Pushed 
534a21c584c7: Pushed 
8b71b84c0598: Pushed 
7703e7dd58ac: Pushed 
af7ed92504ae: Pushed 
1.0: digest: sha256:612a71480f73ed57d4622b478685992ef85447c7c75780c49c7dd4c416fc72d3 size: 2211

// 终端显示以上信息则说明推送镜像成功了
```

### 5. 查看提交的镜像：
* 方法1：打开镜像仓库的网页查看，默认地址为：`http://dockerhubIP:5000/v2/_catalog`,这里的仓库地址为：`http://10.0.0.139:5000/v2/_catalog`，可以看到刚刚上传的镜像名，但是不包含版本号。

![](https://s2.loli.net/2022/06/16/WsQUXyuzgicBJrn.png)

* 方法2：在服务机运行DockerHub时，我们是把容器内存放镜像的文件夹挂载到宿主机的文件夹的`-v /docker/registry/:/var/lib/registry/`,所以我们现在可以查看宿主机上的`/docker/registry/`文件是否有刚刚上传的镜像：
```bashshell
$ cd /docker/registry/docker/registry/v2/repositories/
$ pwd
/docker/registry/docker/registry/v2/repositories
$ ls
lemumsg-api
```
### 6. 在普通用户机拉取私有仓库镜像
```bashshell
// 先把用户机的镜像删掉
// 查看本机的docker 镜像
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED        SIZE
10.0.0.139:5000/lemumsg-api   1.0       d552ded0dac3   4 hours ago    439MB

// 删除lemumsg-api 的镜像
$ docker rmi -f d552ded0dac3

// 从私有仓库拉取镜像
// 命令：docker pull 仓库ip:5000/镜像名:TAG ,如果不加tag标签，默认是拉取latest版本的
$ docker pull 10.0.0.139:5000/lemumsg-api:1.0
1.0: Pulling from lemumsg-api
d7bfe07ed847: Already exists 
113e4be7bf17: Already exists 
635faa39050b: Already exists 
5aa68006bcd1: Already exists 
ee14275eaa84: Already exists 
5e4251489e73: Already exists 
522f332b55ee: Already exists 
7a388718be3a: Already exists 
0514e9b2d8cc: Already exists 
Digest: sha256:612a71480f73ed57d4622b478685992ef85447c7c75780c49c7dd4c416fc72d3
Status: Downloaded newer image for 10.0.0.139:5000/lemumsg-api:1.0
10.0.0.139:5000/lemumsg-api:1.0

// 查看镜像：
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED        SIZE
10.0.0.139:5000/lemumsg-api   1.0       d552ded0dac3   4 hours ago    439MB
```

## 三、Docker Registry 服务日志
### Docker Registry日志的默认位置：
```bashshell
// local 日志驱动的储存位置 /var/lib/docker/containers/容器id/local-logs/ 以container.log 命名。
// 可以使用 docker inspect  容器id | grep -E "LogPath" 查看当前容器的log存放位置
$ docker inspect  2c308894c4d5 | grep -E "LogPath"
"LogPath": "/var/lib/docker/containers/2c308894c4d5eb20613ec3441167c1009fe30e649f91823e0ec79553d237160c/2c308894c4d5eb20613ec3441167c1009fe30e649f91823e0ec79553d237160c-json.log",

// 查看容器引擎日志
$ journalctl -u docker 
```
### 查看系统当前设置的日志驱动
```bashshell
$ docker inspect  -f '{{.HostConfig.LogConfig.Type}}'   容器id
```

## 四、在服务机创建自签发证书，生成用户名和密码进行登录
```bashshell
// 创建证书文件夹
$ mkdir /certs
// 创建registry登录用户配置文件文件夹
$ mkdir /auth
// 生成自签名证书
$ openssl req -x509 -days 365 -subj '/CN=183.234.102.178:55000/' -newkey rsa:4096 -nodes -sha256 -keyout /certs/domain.key -out /certs/domain.crt


// 关于openssl命令中的一些参数说明如下。
● -x509：x509是一个自签发证书的格式；
● -days 365：表示证书有效期；
● 183.234.102.178:55000：表示具体部署Docker Registry本地镜像仓库的地址和端口；
● rsa:2048：是证书算法长度；
● domain.key和domain.crt：就是生成的证书文件。


// 创建一个我们的private registry用户，admin admin 就是账号和密码了。
// 在Docker Registry本地镜像仓库所在的Docker主机上生成自签名证书后，为了确保Docker机器与该Docker Registry本地镜像仓库的交互，还需要生成一个连接认证的用户名和密码，使其他Docker用户只有通过用户名和密码登录后才允许连接到Docker Registry本地镜像仓库。
$ docker run --rm --entrypoint htpasswd registry:2.6.2 -Bbn admin admin >> auth/htpasswd // 管理员帐号
$ docker run --rm --entrypoint htpasswd registry:2.6.2 -Bbn lemu lemu >> auth/htpasswd // 普通账户

// 启动Docker hub镜像
$ docker run -dit -p 5000:5000 --restart=always --name hub \
  -v /auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -v /certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -v /docker/registry/:/var/lib/registry/ \
  registry:2.6.2

// 解释参数
-d：表示容器后台运行
-p：端口映射 --restart=always：可以理解为开机启动。开机：就是启动docker客户端拉。
--name registry：给容器取一个名字，方便识别和记忆 -v:挂在本地文件到容器中。命令格式：hostdir:cdir[:rw|ro] 主机目录:容器目录[:读写权限]
-v pwd/auth:/auth：挂在本地的密码文件夹
-v pwd/certs:/certs：挂在本地的ssl证书文件夹
-e:设置环境变量参数
-e REGISTRY_AUTH：验证方式 
-e REGISTRY_AUTH_HTPASSWD_REALM：验证域名 
-e REGISTRY_AUTH_HTPASSWD_PATH：密码文件路径 
-e REGISTRY_HTTP_TLS_CERTIFICATE：ssl证书文件路径 
-e REGISTRY_HTTP_TLS_KEY：ssl证书文件路径 
最后的registry则是镜像的名字了。

// 配置Docker Registry访问接口
$ sudo mkdir -p /etc/docker/certs.d/183.234.102.178:55000
$ sudo cp certs/domain.crt /etc/docker/certs.d/183.234.102.178:55000

// 在 /etc/docker/daemon.json 新增配置文件中添加 "insecure-registries": [ "183.234.102.178:55000"] 此处IP为私有仓库IP
$ sudo vim /etc/docker/daemon.json 
{
    "registry-mirrors": [
        "https://eoxejquzx.mirror.aliyuncs.com",
        "https://registry.docker-cn.com"
        ],
    "insecure-registries": [
      "10.0.0.139:5000",
      "183.234.102.178:55000"
      ]
}

// 修改完成后重启docker服务,如果重启服务失败，请检查/etc/docker/daemon.json的内容是否正确
$ sudo systemctl daemon-reload 
$ sudo systemctl restart docker.service

// 推送镜像测试
$ docker push 183.234.102.178:55000/lemudevmsg-api
Using default tag: latest
The push refers to repository [183.234.102.178:55000/lemudevmsg-api]
tag does not exist: 183.234.102.178:55000/lemudevmsg-api:latest

// 登录Docker Registry镜像仓库
$ docker login 183.234.102.178:55000
Username: admin
Password: 
WARNING! Your password will be stored unencrypted in /home/lemu-devops/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login///credentials-store

Login Succeeded
```
* 网页验证测试：打开网页`https://183.234.102.178:55000/v2/_catalog`
  * 输入登录名和密码：`admin`

![](https://s2.loli.net/2022/06/20/NuwaBED8hyl1dUS.png)


## 五、Docker镜像仓库的管理和运维
### 1. 确保镜像内容的一致性
在应用的开发、测试和运行等各个阶段，需要确保都使用同一个应用的镜像。一种做法是在每个阶段都用相同dockerfile去生成所需镜像。通常认为，相同的dockerfile可以构件出相同的镜像，而实际上却并非如此。例如下面的dockerfile 部分：
```bashshell
FROM ubuntu
RUN apt-get install –y python
ADD app.py /myapp/app.py
```
首先，`FROM`的基础镜像隐含使用了`latest`版本，在不同时间构建的镜像，可能是不同的版本。即使指明了`ubuntu:20.04`这样的版本号也不保险，因为相同版本的系统可能含有补丁等不尽相同的软件包。

再看`apt-get`这样的命令（类似的还有curl，wget等），往往会从互联网上引入第三方的软件包，版本的一致性就更加无从确定了。还有在ADD语句中添加到镜像里面的文件app.py，取决于构建时的文件版本，也是一个不确定的因素。

从上面的例子可以看到，尽管docker镜像的目的是构造不可更改的应用环境，但由于其构建的时候往往具有不确定的输入，相同dockerfile生成的镜像未必包含相同的内容。因此，最好的方法还是在不同的环境中始终采用相同的镜像（二进制格式），虽然在传输量上比dockerfile要大，但是可以确保镜像的一致性。

### 2. 镜像的传输
很多用户在开发、测试和运维中都使用同一个Registry作为镜像仓库，这种方式比较适合小团队或简单的项目。在其他情况下，最好使用多个Registry以区分不同的用途和安全控制要求。容器镜像管理的参考流程（如下图所示）。

![](https://s2.loli.net/2022/06/20/p5RjSaUWQrMCKoL.png)

* 开发环境的Registry: 主要由开发人员使用，镜像变化频繁。当开发完成后，通过CI系统生成稳定镜像，并同步到测试Registry;

* 测试环境的Registry: 主要由测试人员使用，镜像保持不变。当测试通过后，镜像推送到准生产环境的Registry;

* 准生产环境（Staging）的Registry: 主要由测试和运维人员使用，镜像保持不变。当准生产环境试运行后平稳后，再发布到生产环境的Registry;

* 生产环境的Registry: 发布镜像到生产环境的节点运行。

从开发到生产的整个过程中，符合要求的容器镜像会逐步进入下一级的Registry，最后到达生产系统，从而实现容器镜像的构建-传输-运行（Build-Ship-Run）过程。

### 3. 镜像的权限控制
在企业中，通常有不同的开发团队来负责不同的应用项目，和源代码分项目管理一样，镜像也需要按照项目来存放和管理。由于项目团队中有不同的成员，如项目经理、产品经理、开发、测试和运维等人员，每种人员使用镜像的需求不同，因此可以根据角色分配相应的权限。

例如，测试人员通常只需要镜像的读权限(pull)，开发人员需要读写权限(push/pull)，项目经理除了拥有开发人员的权限之外，还可以增加和删除项目成员，设定他们的角色。

所以每个项目中可有三种角色：项目管理员(project admin)、开发者(developer)和客人(guest)。某些项目，如放在library中的公共镜像, 可以允许匿名访问，即用户不用docker login也可以访问，这样方便某些场景的使用。在整个系统中，还设有系统管理员，具有维护镜像同步策略、用户增删等权限。

需要指出的是，在不同的环境中，某个成员的角色可以不同。例如，在开发环境的registry中，运维人员一般不需要权限（或只需要读权限）；而在生产环境中的Registry，运维人员就需要有读写权限。

暂时还没找到原生Docker Hub设置管理人员角色的相关设置，可能需要接入第三方开源的镜像库管理软件来进行管理，比如：
  * VMware Harbor（简称Harbor）项目是由VMware中国研发团队开发的开源容器镜像仓库系统,主要特性：
    * 基于角色的访问控制
    * 镜像复制
    * Web UI管理界面
    * 可以集成LDAP或AD用户认证系统
    * 审计日志
    * 提供RESTful API以提供外部客户端调用
    * 镜像安全漏洞扫描（从v1.2版本开始集成了Clair景象扫描工具）
  * Sonatype Nexus是一个软件仓库管理器，主要有2.X和3.X两个大版本，主要特性：
    * 部署简单，通过启动一个容器即可完成
    `docker run -d --name nexus -p 5000:5000 -p 8081:8081 sonatype/ docker.io/sonatype/nexus3`
    * 支持TLS安全认证
    * 提供Web UI管理界面
    * 支持代理仓库（Docker Proxy），可以将到Nexus镜像仓库的操作代理到另一个远程镜像库
    * 支持仓库组（Docker Group），可以把多个仓库组合成一个地址提供服务
    * 除了支持Docker镜像，还支持对其他软件仓库的管理，例如：Yum、Npm等。目前不支持APK（alpine系统软件仓库）
  * SUSE Portus是另一个开源镜像库，其特点包括：
    * 基于组（Team）和命名空间（Namespace）的细粒度访问权限控制
    * Web UI管理界面
    * 可以集成LDAP用户认证系统，也支持OAuth
    * 审计日志
    * 提供RESTful API，以供外部客户端调用
    * 镜像安全漏洞扫描（集成Clair镜像扫描工具）

* 以上几种方案的特性对比

| 方案特性          | Docker Registry | VMware Harbor                | Sonatype Nexus              | SUSE Portus |
| ----------------- | --------------- | ---------------------------- | --------------------------- | ----------- |
| 系统复杂度        | 简单            | 复杂                         | 简单                        | 一般        |
| 配置难易度        | 简单            | 复杂                         | 一般                        | 一般        |
| Web UI管理界面    | 无              | 有                           | 有                          | 有          |
| 与外部LDAP/AD集成 | 无              | 有                           | 有                          | 有          |
| 访问权限控制      | 弱              | 强                           | 弱                          | 强          |
| 镜像复制          | 无              | 支持复制到另一个Harbor镜像库 | 支持Proxy代理到另一个镜像库 | 弱          |
| 镜像扫描          | 无              | 可集成Clair                  | 无                          | 可集成Clair |


## 六、Docker Hub部署到Gitlab仓库
### 部署Gitlab仓库
1. 拉取gitlab-ce的镜像
```bashshell
$ docker pull gitlab/gitlab-ce
Using default tag: latest
latest: Pulling from gitlab/gitlab-ce
7b1a6ab2e44d: Pull complete 
6c37b8f20a77: Pull complete 
f50912690f18: Pull complete 
bb6bfd78fa06: Pull complete 
2c03ae575fcd: Pull complete 
839c111a7d43: Pull complete 
4989fee924bc: Pull complete 
666a7fb30a46: Pull complete 
Digest: sha256:5a0b03f09ab2f2634ecc6bfeb41521d19329cf4c9bbf330227117c048e7b5163
Status: Downloaded newer image for gitlab/gitlab-ce:latest
docker.io/gitlab/gitlab-ce:latest
```
2. 启动镜像
```bashshell
$ docker run -d \
--hostname 10.0.0.139 \
-p 8443:443 \
-p 9980:80 \
-p 2222:22 \
--name gitlab \
--restart always \
-v /usr/local/gitlab/config:/etc/gitlab \
-v /usr/local/gitlab/logs:/var/log/gitlab \
-v /usr/local/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce
```
3. 查看镜像运行情况：
```bashshell
$ docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS                            PORTS                                                                                                                   NAMES
55fb5717a53c   gitlab/gitlab-ce   "/assets/wrapper"        3 seconds ago   Up 2 seconds (health: starting)   0.0.0.0:2222->22/tcp, :::2222->22/tcp, 0.0.0.0:9980->80/tcp, :::9980->80/tcp, 0.0.0.0:8443->443/tcp, :::8443->443/tcp   gitlab
```


4. 修改配置文件
* **提示**：等待docker运行gitlab一小段时间后在操作运行状态为`Up 24 minutes (healthy) `，否则可能出现文件找不到

```bashshell
// 修改gitlab.rb文件
$ vim /usr/local/gitlab/config/gitlab.rb 

// 找到external_url，默认是被注释的，要打开，并填写暴露出去的http://ip:port，port为你启动时指定的，我们这里使用9980作为端口；最后加上ssh协议下使用的IP和端口(这里的端口是你启动时指定的，我们这里是2222)，最后保存并退出

external_url 'http://10.0.0.139:9980'
gitlab_rails['gitlab_shell_ssh_port'] = '2222'
```
5. 停止并移除之前启动的gitlab
```bashshell
$ docker stop 55fb5717a53c
55fb5717a53c
$ docker rm -f  55fb5717a53c
55fb5717a53c

// 重新启动gitlab
// 这里要将容器端口改为9980.
$ docker run -d \
--hostname 10.0.0.139 \
-p 8443:443 \
-p 9980:9980 \
-p 2222:22 \
--name gitlab \
--restart always \
-v /usr/local/gitlab/config:/etc/gitlab \
-v /usr/local/gitlab/logs:/var/log/gitlab \
-v /usr/local/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce

// 等待gitlab初始化完成，就可以访问了，首次需要更改root账户的密码
$ docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
Password: WVXNS3kqyxBRHlTB2drp4/tTThzoWzXe/vm2RTumS7M=

// 预设用户为root，打开浏览器输入：http://10.0.0.139:9980/ 或 http://183.234.102.178:9980/ 登录gitlab之后修改就好，修改后的密码为：Lemu@1215

// 最后还需要添加机器的ssh密钥到gitlab上
```
### 安装Gitlab-Runner
1. 创建一个空白的项目：CICD_Demo进行测试：

![](https://s2.loli.net/2022/06/22/8Mrfjd42ZLFkhln.png)

2. 查看当前项目的Gitlab-Runner令牌（Token）,打开方式：项目-设置-CICD-Runner

![](https://s2.loli.net/2022/06/22/Wjs6fPpqUkQLKRB.png)

3. 拉取gitlab-Runner镜像并启动
```bashshell
$ docker run -d --name gitlab-runner --restart always -v /srv/gitlab-runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner:latest
```

4. 注册并配置Runner
```bashshell
$ docker exec gitlab-runner gitlab-runner register -n \
--url http://10.0.0.139:9980/ \
--registration-token yuiTZyXpDQwPo98vd2XP \
--tag-list test \
--executor docker \
--docker-image docker \
--docker-volumes /root/.m2:/root/.m2 \
--docker-volumes /root/.npm:/root/.npm \
--docker-volumes /var/run/docker.sock:/var/run/docker.sock \
--description "test runner"
```
通过以上命令后，就创建成功runner啦，这时候我们去GitLab中我们创建Runner的区域刷新就能看到了

![](https://s2.loli.net/2022/06/22/I3SWOnCdiAvLg8G.png)

5. 修改Runner配置文件（非必须）
```bashshell
$ vim /srv/gitlab-runner/config/config.toml

// 在volumes配置下方增加一行配置，防止Runner重复拉取镜像
pull_policy = "if-not-present"

// 最后重启Runner
$ docker restart gitlab-runner
```
