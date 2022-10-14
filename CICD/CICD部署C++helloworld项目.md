# GItlab-ci CI/CD部署C语言helloworld项目

### 安装Gitlab-runner命令行
* 根据所有的系统类型参考[Install GitLab Runner](https://docs.gitlab.com/runner/install/)官网的文档进行安装；
* 我使用的是Ubuntu系统的安装方式[install](https://about.gitlab.cn/blog/2021/12/07/runner-ubuntu/);
  1. 添加官方 GitLab 存储库：
  ```sh
  $ curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
  ```
  2. 安装最新版本的 GitLab Runner，或跳到下一步安装特定版本：
  ```sh
  $ sudo apt-get install gitlab-runner
  ```
  3. 要安装特定版本的 GitLab Runner：
  ```sh
  $ apt-cache madison gitlab-runner
  $ sudo apt-get install gitlab-runner=10.0.0
  ```
  4. deb文件安装；
  ```sh
  $ curl -LJO "https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_amd64.deb"
  $ ls -ltr
  # 安装
  $ dpkg -i gitlab-runner_amd64.deb
  ```

### Runner 的注册
Runner 安装完毕，在真正使用之前需要先进行注册。注册的目的是让 Runner 和GitLab 实例建立链接通道，当GitLab 实例中的项目有 CI/CD Pipeline 需要执行的时候，就会通过这个注册的 Runner 来执行。极狐GitLab Runner 的注册很简单，通过```gitlab-runner register```命令即可。

1. 创建一个CICD_HelloWold的项目：

![cicd](https://img-blog.csdnimg.cn/img_convert/e14d8f0ec73eed9656d6fa6f53b80235.png)

2. 获取Gitlab实例的```URL```和```Token```，这些内容可以通过项目的 Setting –> CI/CD –> Runner 选项来获取，如下图所示：

![cicd2](https://img-blog.csdnimg.cn/img_convert/22272b4e5082603df8ad27c3084e32af.png)

3. 检查是否安装了Runner
```sh
$ gitlab-runner list                                              1 ↵  
Runtime platform                                    arch=amd64 os=linux pid=1125737 revision=98daeee0 version=14.7.0
Listing configured runners                          ConfigFile=/home/lemu-devops/.gitlab-runner/config.toml
```
4. 可以看到，当前没有可使用的 Runner，接下来开始注册
```sh
$ gitlab-runner register                                        126 ↵  
Runtime platform                                    arch=amd64 os=linux pid=1132198 revision=98daeee0 version=14.7.0
WARNING: Running in user-mode.                     
WARNING: The user-mode requires you to manually start builds processing: 
WARNING: $ gitlab-runner run                       
WARNING: Use sudo for system-mode:                 
WARNING: $ sudo gitlab-runner...                   
                                                   
Enter the GitLab instance URL (for example, https://gitlab.com/):
https://gitlab.com/ # 从gitlab界面的Setting中获取url
Enter the registration token:
hvuZUyUtuvyuSAnStkqw # 从gitlab界面的Setting中获取url
Enter a description for the runner:
[lemudevops]: test runner # runner描述
Enter tags for the runner (comma-separated):
build # 表明这个runner只能执行tag为build的jod。
Registering runner... succeeded                     runner=hvuZUyUt
Enter an executor: docker-ssh, shell, ssh, virtualbox, docker+machine, docker-ssh+machine, docker, parallels, kubernetes, custom:
shell # 按实际平台填写，linux这里选shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 
```
5. 激活Runner
注册完了可能还需要激活，这时我们可以看下面板，如果有个黑色的感叹号，这说明Runner注册成功了，但是尚未激活（如果是绿色的说明已经激活，本步骤跳过）。

![jihuo](https://img-blog.csdnimg.cn/img_convert/1fde73ef98a446ac96b5b7fe6f62092d.png)

激活方法是本地运行的：
```sh
$ gitlab-runner verify                                                 
Runtime platform                                    arch=amd64 os=linux pid=1137496 revision=98daeee0 version=14.7.0
WARNING: Running in user-mode.                     
WARNING: The user-mode requires you to manually start builds processing: 
WARNING: $ gitlab-runner run                       
WARNING: Use sudo for system-mode:                 
WARNING: $ sudo gitlab-runner...                   
                                                   
Verifying runner... is alive                        runner=G34swjUP
```
再刷新下网页，黑色的变绿色就说明激活成功了。

![jihuo2](https://img-blog.csdnimg.cn/img_convert/f54a529fda9e782096d683049be4b97c.png)

如果没有激活成功需要运行一下命令重新启动Runner.
```sh
$ gitlab-runner verify
$ gitlab-runner restart
```

### Runner的使用
1. 创建YAML的配置文件，不用手动创建，回到Gitlab项目根目录，选择配置CI/CD-编辑-创建新的CI/CD流水线-浏览模板（新的窗口打开）-选择C++.gitlab-ci.yml-复制配置内容-回到根项目的配置粘贴复制的配置内容。

![a](https://img-blog.csdnimg.cn/img_convert/1a2496ad33a8bf1923c2b9d70ebe04ac.png)

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-JXtK9UYd-1644892384425)(https://raw.githubusercontent.com/Kizai/Images/main/202202132057935.png)\]](https://img-blog.csdnimg.cn/4c7825aa79c24a5ca86223590f97521e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS0laQUk=,size_20,color_FFFFFF,t_70,g_se,x_16)





![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-SrnXjgC5-1644892384426)(https://raw.githubusercontent.com/Kizai/Images/main/202202132058901.png)\]](https://img-blog.csdnimg.cn/354be620341947d09fab493c585160cc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS0laQUk=,size_20,color_FFFFFF,t_70,g_se,x_16)


![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-35BkWM3y-1644892384426)(https://raw.githubusercontent.com/Kizai/Images/main/202202132059530.png)\]](https://img-blog.csdnimg.cn/801e759e871b431e8fe4659ab065d0f2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS0laQUk=,size_20,color_FFFFFF,t_70,g_se,x_16)


2. 拉取cicd_helloworld的项目到本地，创建helloworld.cpp
```c++
#include <stdio.h>
int main()
{
   printf("Hello, World!");
   return 0;
}
```
3. 创建test的测试脚本：
```sh
#!/bin/bash

g++ helloworld.cpp -o helloworld
wait
./helloworld
```
4. 修改.gitlab-ci.yml文件
```yml
# To contribute improvements to CI/CD templates, please follow the Development guide at:
# https://docs.gitlab.com/ee/development/cicd/templates.html
# This specific template is located at:
# https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/C++.gitlab-ci.yml

# use the official gcc image, based on debian
# can use verions as well, like gcc:5.2
# see https://hub.docker.com/_/gcc/

image: gcc

build:
  stage: build
  # instead of calling g++ directly you can also use some build toolkit like make
  # install the necessary build tools when needed
  # before_script:
  #   - apt update && apt -y install make autoconf
  script:
    - echo "Compiling the code..."
    - g++ helloworld.cpp -o helloworld
    - echo "Compile complete."
  artifacts:
    paths:
      - helloworld
      # depending on your build setup it's most likely a good idea to cache outputs to reduce the build time
      # cache:
      #   paths:
      #     - "*.o"

# run tests using the binary built before
test:
  stage: test
  script:
    - chmod +x helloworld.sh
    - ./helloworld.sh
```
1. 提交(PUSH))代码触发流水线自动运行
```sh
git add .
git commit -m "helloworld"
git push origin main
```
6. 回到网页打开流水线，会看到流水线正在执行,如果build没有完成的话，还没有到test的步骤。

![p1](https://img-blog.csdnimg.cn/img_convert/fd4fab328d4f429b0c9c8909bc83ceae.png)

![p2](https://img-blog.csdnimg.cn/img_convert/a3aa6eff3ce54e2758bea698c047e185.png)

查看build的信息

![pp1](https://img-blog.csdnimg.cn/img_convert/89f394481a276fcd883edb6ac841d2b6.png)

Build编译通过后会自动运行test测试。

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-Jlv2GZg6-1644892384428)(https://raw.githubusercontent.com/Kizai/Images/main/202202132214880.png)\]](https://img-blog.csdnimg.cn/6eece7f5a8924fecbe887844478e18f2.png)


![p4](https://img-blog.csdnimg.cn/img_convert/ba8202dd436f521002ee47581b064cc0.png)

test的信息

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-2PZHMSNZ-1644892384429)(https://raw.githubusercontent.com/Kizai/Images/main/202202132217095.png)\]](https://img-blog.csdnimg.cn/c3e17edc09814b6585091bec106ee5d7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAS0laQUk=,size_20,color_FFFFFF,t_70,g_se,x_16)
以上只是CI/CD的一个测试Demo，主要是让入门开发者了解CI/CD的一个自动流水线测试的一个过程，方便大家去理解。

### 参考链接
- [gitlab](https://docs.gitlab.com/ee/user/project/pages/getting_started/pages_ci_cd_template.html#create-a-pages-website-by-using-a-cicd-template)