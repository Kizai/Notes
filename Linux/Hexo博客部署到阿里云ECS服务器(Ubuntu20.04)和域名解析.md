### 背景
一开始我把```Hexo```博客部署到```Github```上,其实都是博客最入门的做法，如果没有把自己做的博客部署到自己购买的服务器上再加上域名解析，我觉得这个过程是不完整的，所以我把我自己部署网站的整个过程都记录下来，希望可以对即将部署自己网站的小伙伴们，提供一些参考意见。

## 服务器配置：
### 服务器购买推荐:
1. [阿里云服务器](https://www.aliyun.com/)，阿里云的服务器每年在双十一都会有活动，优惠很大。
2. 推荐使用"轻量级应用共享型ECS服务器",推荐安装系统```Ubuntu 20.04```，或者你自己最熟悉的系统。

### 远程连接服务器
1. 进入阿里云→云服务器管理控制台→远程连接，设置好root密码重启电脑生效。
2. 配置ssh远程连接服务器，在云服务器控制台→进入服务器实例→安全组→配置规则→出方向→快速添加→选中SSH(22)，按确定即可！！

![22](https://img-blog.csdnimg.cn/img_convert/1c2d9e3ba0570104f493e14c2c0b99a9.png)

3. 假设我服务器公网```IP```地址就是```20.21.11.11```，在本机电脑使用```ssh root@20.21.11.11```远程登陆服务器，连接成功后输入自己刚刚设置好的云服务器的密码，就可以成功登录登录到云服务器啦。

### 在云服务器中创建新用户
#### 创建新用户```kizai```

```bash
adduser kizai # 这个会提示需要重新设置一个新的登录密码
chmod 740 /etc/sudoers
vim /etc/sudoers
```

找到如下```root ALL:(ALL:ALL) ALL```后，添加一行
```bash
kizai ALL:(ALL:ALL) ALL # kizai替换成自己的用户名
```

获取```root```权限
```bash
sudo passwd kizai
```

### 切换到```kizai```用户
切换到新建的用户后(后面都是以此用户和```~```目录下工作)，安装一些常用的软件。
```bash
su kizai
cd ~
sudo apt-get update
sudo apt-get upgrade
sudo apt install vim git htop screenfetch curl wget tmux # 看自己的需求来安装
```
### 配置```SSH```
创建```~/.ssh```和```authorized_keys```文件,赋予权限。
```bash
mkdir ~/.ssh
vim ~/.ssh/authorized_keys #名字不能错
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh/
```

### 本机电脑创建```ssh```公钥
切回到本机电脑，需要生成一个本机电脑的```ssh```公钥，放好服务器的```.ssh/authorized_keys```文件里面。如果本机电脑已经生成公钥可以忽略我这一步。window系统这边建议先装一个叫```Window Terminal```的软件，以下的操作基于这个软件。
```bash
ssh-keygen.exe # 一路按确定就好，会有一个路径提示给你在哪存放了id_rsa.pub
cat C:\Users\KIZAI\.ssh\id_rsa.pub # 根据你的路径修改，直接复制你的公钥到你的剪贴板备用。 
```
再回到服务器端把公钥加到```authoried_keys```文件中。
断开服务器连接，再使用```ssh kizai@20.21.11.11```就可以不需要输登录密码就可以成功连接了。

### 配置Git
创建```Git```裸库```blog.git```,和工作目录```blog```（存放解析后```html```文件）。
```bash
cd ~
mkdir blog
git init --bare blog.git
vim blog.git/hooks/post-receive # 创建新文件
```

在```post-receive```添加```hook```钩子。
```bash
#!/bin/sh
git --work-tree=/home/kizai/blog --git-dir=/home/kizai/blog.git checkout -f
```

添加执行权限
```bash
chmod +x blog.git/hooks/post-receive
```
可以执行```git clone kizai@20.21.11.11:/home/kizai/blog.git```下载云服务的文件到本机。

### 安装```nginx```
安装 nginx 和修改对应配置文件
```bash
sudo apt install
sudo vim /etc/nginx/sites-available/default
```

找到
```bash
# include snippets/snakeoil.conf;

root /var/www/html; 
```

替换
```bash
# include snippets/snakeoil.conf;

root /home/kizai/blog;
```

执行 ```service nginx status```查看```nginx```状态，其默认状态是运行中（服务开始了） + 开机自启 （若是没有，需要执行此状态 ）；如果不熟悉```nginx```的常用命令可以参考[9 Popular Nginx Commands You Should Know](https://www.keycdn.com/support/nginx-commands)。

若此刻直接访问云服务器的公网```IP```不会显示任何内容，因为云服务器的```80```端口还没被打开。
回到云服务器控制管理中心，在云服务器控制台→进入服务器实例→安全组→配置规则→入方向→快速添加→选中HTTP(80)，按确定即可！！
![80](https://img-blog.csdnimg.cn/img_convert/6e59185d6633ef4d62bd0653dff05072.png)

重新输入服务器公网```IP```就可以看到```nginx```欢迎界面，因为此时```/home/kizai/blog```文件夹为空，没有任何```html```文件。

## 本机配置:
### 创建```hexo```文件夹
在本地创建一个```kizaiblogtest```的文件夹，用来最小化验证部署是否正确。
```bash
cd ~
hexo init kizaiblogtest     # 创建和初始化kizaiblogtest文件夹
cnpm install                # 安装插件
cnpm install hexo-deployer-git  # hexo deploy 部署插件
```

修改```kizaiblogtest```根目录下的配置文件```_config.yml```，末尾修改为
```yml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: kizai@20.21.11.11:/home/kizai/blog.git
  branch: master                           
  message: '站点更新:{{now("YYYY-MM-DD HH:mm:ss")}}'
```

### 部署到云服务器
在```kizaiblogtest```文件夹中，使用终端进行部署
```bash
hexo clean && hexo g # 清理 生成html文件
hexo deploy && hexo  # 部署，会自动将生成的 html 文件， push 到阿里云服务器的 /home/kizai/blog 文件夹中
```
然后重启阿里云服务器后，浏览器输入服务器公网```IP```，即可网页看到解析后的部署网页 “Hello World”。

最后替换为自己的真实博客文件夹的```_config.yml```文件末尾处替换为如上，重新执行```hexo deploy```部署即可成功；建议使用```Github```或者```Gitee```把之前的工程文件托管起来做好备份。

## 域名解析
上面的步骤只是把博客部署云服务器上了，但是访问它还是需要用到公网```IP```，这访问也太难受了，所以我们要使用域名的方式来访问网站，那怎么把域名绑定到服务器上呢？其实比较简单，看步骤！
1. 在阿里云或者其他平台购买域名，根据自己的想法来设计自己的域名和它的后缀。购买完需要实名认证。
2. 回到阿里云控制台搜索"云解析DNS"，进入云解析DNS→域名解析→看到自己的域名后面有一个解析设置→点击解析设置，可参考以下设置：

![ipdns](https://img-blog.csdnimg.cn/img_convert/212fd7a6ebfa842cf34658430eb0fb3a.png)

3. 等个几分钟，使用注册好的域名就可以访问自己的博客啦！
4. 在中国地区购买的服务器和域名是需要备案的，大家绑定好域名记得做好备案工作喔！不然网站是正常访问不了的。

欢迎大家访问我的小破站[kizaiblog](http://www.kizaiblog.cn/)


