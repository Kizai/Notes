# Ubuntu 20.04 安装VirtualBox，并在VirtualBox运行Win10
[VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)是开源的跨平台虚拟化软件，可让您同时运行多个来宾操作系统（虚拟机）。 通常，桌面用户将Virtualbox用作测试和开发环境。

在这里会使用两种安装方式：

- 从标准Ubuntu存储库。
- 从Oracle存储库。

Ubuntu版本存储库中可用的VirtualBox软件包可能不是最新版本。 Oracle存储库始终包含最新发布的版本。

### 从Ubuntu存储库安装VirtualBox
从Ubuntu存储库安装VirtualBox是一个简单的过程。 以超级用户或具有```sudo```权限的用户身份运行以下命令，以更新程序包索引并安装VirtualBox和Extension Pack：
```sh
sudo apt update
sudo apt install -y virtualbox virtualbox-ext-pack
```
这样Ubuntu就成功安装了VirtualBox,然后就可以使用它了。

### 从Oracle存储库安装VirtualBox
使用以下命令导入Oracle公共密钥：
```sh
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
```
这两个命令都应输出OK，这意味着密钥已成功导入，并且来自此存储库的软件包将被视为受信任。

将VirtualBox APT存储库添加到您的系统：
```sh
echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | \
     sudo tee -a /etc/apt/sources.list.d/virtualbox.list
```
```$(lsb_release -cs)```打印Ubuntu代号。 例如，如果您Ubuntu版本20.04，则命令将显示```focal```。

更新软件包列表并安装最新版本的VirtualBox：
```sh
sudo apt update
sudo apt install -y virtualbox-6.1
```
### 启动Virtualbox并安装win10系统
注：**启动VirtualBox之前需要保证您电脑本地有Win10的镜像，如果没有镜像先去微软官方下载到本地磁盘。**

1. 可以通过终端输入```virtualbox```或单击VirtualBox图标（Activities -> VirtualBox）从命令行启动VirtualBox。

![1](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061536801.png)

2. 点击蓝色的New图标或者点击Machine->New,会弹出一个Create Virtual Machine的窗口，安装Win10可以按照下图设置，Name这个看个人喜欢。

![2](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061540366.png)

3. 设置内存大小

![3](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061541175.png)

4. 创建一个虚拟硬盘

![4](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061541934.png)

![5](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061541728.png)

![6](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061542057.png)

![7](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061542336.png)

5. 点击Start会弹出一个窗口

![8](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061542630.png)

6. 这个窗口选择系统镜像的位置

![9](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061550743.png)

7. 点击Choose

![10](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061552544.png)

8. win10 安装步骤

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061555867.png)

9. 选择我没有产品密钥

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061555317.png)

10. 选择windows 10专业版

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061555604.png)

11. 点击接受，下一步

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061555765.png)

12. 选择自定义

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061556035.png)

13. 选择驱动器0下一步

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061556650.png)

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061556467.png)

14. 配置好等待重启，继续配置

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061601748.png)

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061601707.png)

15. 跳过

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061602633.png)

16. 针对个人

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061603615.png)

17. 脱机账户

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061604801.png)

18. 有限的体验

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061604223.png)

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061604606.png)

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061605830.png)

19. 否

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061605119.png)

20. 拒绝

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061605488.png)

21. 全部选择否，接受

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061606347.png)

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061606037.png)

22. 进入系统桌面

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061607695.png)

23. 查看ip地址，键盘按住win+R输入CMD，打开终端再输入ipconfig,可以看到ip地址不在同一个子网下，需要修改虚拟机的网络配置。

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061609209.png)

24. 点击VirtualBox顶部菜单栏的Machine->Settings->NetWork,按照下图设置，注意网卡型号可能不一样。

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061609212.png)

25. 再查看ip地址是否更新了

![](http://imagesurl.s3-website.cn-northwest-1.amazonaws.com.cn/lemu_images/202201061609246.png)

26. 以上就是在Ubuntu安装VirtualBox并运行Win10的全部步骤。

### 在 VirtualBox 上安装 Amazon Linux 2
1. 下载 Amazon Linux VDI 镜像,[下载地址](https://cdn.amazonlinux.com/os-images/2.0.20220316.0/virtualbox/)。
要安装在 Virtualbox 上的 AMI 映像不提供 ISO 格式，而是直接以虚拟映像的形式提供，具体取决于您使用的虚拟机平台类型。目前，Amazon 系统映像可用于 Hyper-V、KVM、VMware 和 VirtualBox。

![vdi](https://s2.loli.net/2022/03/21/rDGOMcBNtYeQKTA.png)

2. Seed.ISO 文件——root 用户密码
默认情况下，我们是无法访问或登录Amazon Linux 2 根用户，因为我们必须再下载一个名为 Seed.ISO 的文件。

![iso](https://s2.loli.net/2022/03/21/k69eNfZKVymBL3r.png)

3. 创建亚马逊 Linux 虚拟机,点击 VirtualBox 上的NEW按钮，创建一个新的虚拟机。

![new](https://s2.loli.net/2022/03/21/9xUWovYl3R2Q7wk.png)

4. 在 Virtualbox 上命名操作系统，AWS_Linux，其余的内容（例如类型和版本）将自动被选中。

![](https://s2.loli.net/2022/03/21/BMkNKT7jubcfoZs.png)

5. 设置 RAM - 选择您要分配的内存量 - 这里选择4G。

![](https://s2.loli.net/2022/03/21/LCse6ZbTmtWJigI.png)

6. 由于我们已经下载了 Amazon Linux VDI 映像，因此我们不需要创建新映像。相反，我们使用我们系统上的那个。选择“使用现有的虚拟硬盘文件”，然后单击文件夹图标。

![](https://s2.loli.net/2022/03/21/FY4IRcLXUJOliZC.png)

![](https://s2.loli.net/2022/03/21/tpsnDcdjrbqUHPv.png)

7. 将 Seed.ISO 文件添加到 VirtualBox VM，从左侧选择创建的虚拟机，然后点击菜单中的setting按钮。

![](https://s2.loli.net/2022/03/21/pCFJOBNojawrX9q.png)

8. 从settings- 单击Storage并选择控制器下的Empty CD 图标：IDE。然后单击Attributes CD 图标和Choose/Create a Virtual Optical Disk.。


9. 单击添加按钮，文件资源管理器将打开，选择下载的种子 ISO 文件。之后，它将出现在显示区域中。也从那里选择它并点击选择按钮。

![](https://s2.loli.net/2022/03/21/xp8WGUJhPONRqZK.png)

10. 点击OK按钮

![](https://s2.loli.net/2022/03/21/HCraDV6FXI2Yh4P.png)

