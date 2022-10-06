# Linux find命令：在目录中查找文件
## 参考网站：[A Guide to the Linux “Find” Command](https://www.booleanworld.com/guide-linux-find-command/)

find 是 Linux 中强大的搜索命令，不仅可以按照文件名搜索文件，还可以按照权限、大小、时间、inode 号等来搜索文件。但是 find 命令是直接在硬盘中进行搜索的，如果指定的搜索范围过大，find命令就会消耗较大的系统资源，导致服务器压力过大。所以，在使用 find 命令搜索时，不要指定过大的搜索范围。
### find 命令基本信息：
* 命令名称：find。
* 英文原意：search for files in a directory hierarchy.
* 所在路径：/bin/find。
* 执行权限：所有用户。
* 功能描述：在目录中查找文件。

### find基本命令格式：
```bash
find [搜索路径] [选项] [搜索内容]
```
>find 是比较特殊的命令，它有两个参数：
>> * 第一个参数用来指定搜索路径;
>> * 第二个参数用来指定搜索内容。

### 查找所有文件和目录：
想象一下，您想列出给定路径的所有目录和文件。例如，我想列出指定文件夹的文件和目录，可以运行：
```bash
find /path/

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc/DevOps_Doc$ find NAS/
NAS/
NAS/.gitkeep
NAS/NAS学习文档笔记
NAS/NAS学习文档笔记/FileSystem.md
NAS/NAS学习文档笔记/lect04_FileSystem.pdf
NAS/NAS学习文档笔记/RAID.md
NAS/NAS学习文档笔记/What_is_NAS.md
NAS/NAS技术文件
NAS/NAS技术文件/NAS使用手册.md
NAS/NAS技术文件/Syno_HIG_DS1520_Plus_enu.pdf
NAS/NAS技术文件/Syno_UsersGuide_NAServer_chs.pdf
NAS/NAS操作记录文档
NAS/NAS操作记录文档/NAS-Lemu_Tech文件夹目录.txt
NAS/NAS操作记录文档/NAS创建共享文件夹操作记录.md
NAS/NAS操作记录文档/NAS新增存储空间操作.md
NAS/NAS操作记录文档/NAS添加用户操作记录.md
NAS/NAS操作记录文档/NAS添加用户群组.md
NAS/NAS管理员文档
NAS/NAS管理员文档/lsblk命令.md
NAS/NAS管理员文档/NAS存储空间与用户帐号分配.md
NAS/NAS管理员文档/NAS的安装与配置.md
NAS/NAS管理员文档/NAS的需求.md
NAS/NAS管理员文档/NAS管理员文档.md
NAS/NAS红盘报价单.md
NAS/NAS遇到的问题
NAS/NAS遇到的问题/NAS硬盘无法初始化.md
NAS/README.md
```
> 如果您要列出多个目录的内容，您可以这样做：
```bash
find /path1 /path2 /path3

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find Router_Doc/ Network_Config/ Cloud_Config/
Router_Doc/
Router_Doc/.gitkeep
Router_Doc/FW70产品使用说明.pdf
Router_Doc/FW70技术参数-.pdf
Router_Doc/R50产品使用说明书V3.0.pdf
Router_Doc/R50系列多口路由器-规格书-抽屉式doc.pdf
Router_Doc/Serillink SLK-4008工业级4G 3G路由器规格书.pdf
Router_Doc/slk-r4008工业4G路由器说明书_v1.0.pdf
Router_Doc/讯悠EC21工业路由器规格书ASR.pdf
Network_Config/
Network_Config/lemu_network.md
Network_Config/设备MAC与静态IP地址绑定.md
Cloud_Config/
Cloud_Config/.gitkeep
Cloud_Config/readme.md
```
> 要列出当前工作目录的内容，请使用句点（）作为路径：```.```
```bash
find .
# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc/Network_Config$ find .
.
./lemu_network.md
./设备MAC与静态IP地址绑定.md
```
### 按照文件名搜索
```bash
find [搜索路径] [选项] [搜索内容]
```
> 选项：
>> * -name: 按照文件名搜索；
>> * -iname: 按照文件名搜索，不区分文件名大小；
>> * -inum: 按照 inode 号搜索；

> 这是find最常用的方法之一：
```bash
# 例如：我查找我文件夹中所有包含.md后缀的文件
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/ -name "*.md"
DevOps_Doc/DevOps工作进度详情.md
DevOps_Doc/NAS/NAS学习文档笔记/FileSystem.md
DevOps_Doc/NAS/NAS学习文档笔记/RAID.md
DevOps_Doc/NAS/NAS学习文档笔记/What_is_NAS.md
DevOps_Doc/NAS/NAS技术文件/NAS使用手册.md
DevOps_Doc/NAS/NAS操作记录文档/NAS创建共享文件夹操作记录.md
DevOps_Doc/NAS/NAS操作记录文档/NAS新增存储空间操作.md
DevOps_Doc/NAS/NAS操作记录文档/NAS添加用户操作记录.md
DevOps_Doc/NAS/NAS操作记录文档/NAS添加用户群组.md
DevOps_Doc/NAS/NAS管理员文档/lsblk命令.md
DevOps_Doc/NAS/NAS管理员文档/NAS存储空间与用户帐号分配.md
DevOps_Doc/NAS/NAS管理员文档/NAS的安装与配置.md
DevOps_Doc/NAS/NAS管理员文档/NAS的需求.md
DevOps_Doc/NAS/NAS管理员文档/NAS管理员文档.md
DevOps_Doc/NAS/NAS红盘报价单.md
DevOps_Doc/NAS/NAS遇到的问题/NAS硬盘无法初始化.md
DevOps_Doc/NAS/README.md
DevOps_Doc/Shell/Shell_Example/cron定时脚本设计文档.md
DevOps_Doc/Shell/Shell_Example/Cron语法.md
DevOps_Doc/Shell/Shell_Example/df-sed-awk-du-grep.md
DevOps_Doc/Shell/Shell_Example/find指令.md
DevOps_Doc/Shell/Shell_Example/NAS监视器脚本操作记录.md
DevOps_Doc/Shell/Shell_Example/NAS监视器设计文档.md
DevOps_Doc/Shell/Shell_Example/Shell监控用户操作轨迹.md
DevOps_Doc/Shell/Shell_Example/Shell监视器设计文档.md
DevOps_Doc/Shell/Shell_Example/Shell脚本监控磁盘存储预警设计文档.md
DevOps_Doc/Shell/Shell_Example/Shell脚本监控磁盘存储预警设计文档Shell脚本监控磁盘存储预警操作文档.md
DevOps_Doc/Shell/Shell_Notes.md
DevOps_Doc/System_Backup/AWS_S3存储管理设计文档.md
DevOps_Doc/System_Backup/系统镜像备份与恢复设计文档.md
DevOps_Doc/Work_Plan.md
DevOps_Doc/每日工作计划.md
```
> 如果您想要搜索包含八个字母的文件和目录，请运行：
```bash
find /usr -name '????????'

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/ -name '????????'
DevOps_Doc/.gitkeep
DevOps_Doc/NAS/.gitkeep
DevOps_Doc/NAS/NAS管理员文档
DevOps_Doc/NAS/NAS遇到的问题
DevOps_Doc/Shell/.gitkeep
DevOps_Doc/Shell/20210728
DevOps_Doc/Shell/20210728/test.txt
DevOps_Doc/Shell/20210728/yesno.sh
DevOps_Doc/System_Backup/.gitkeep
```
### 搜索文件或目录
到目前为止，我们所看到的所有示例都会返回文件和目录。但是，如果您需要仅搜索文件或目录，则可以使用该交换机。最常见的参数是：-type [参数]
> 参数:
>> * f： 通用类型的文件
>> * d： 目录
>> * l：符号链接

> 例如要查找目录中的所有文件，可以运行：
```bash
find /path -type f

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/NAS/ -type f
DevOps_Doc/NAS/.gitkeep
DevOps_Doc/NAS/NAS学习文档笔记/FileSystem.md
DevOps_Doc/NAS/NAS学习文档笔记/lect04_FileSystem.pdf
DevOps_Doc/NAS/NAS学习文档笔记/RAID.md
DevOps_Doc/NAS/NAS学习文档笔记/What_is_NAS.md
DevOps_Doc/NAS/NAS技术文件/NAS使用手册.md
DevOps_Doc/NAS/NAS技术文件/Syno_HIG_DS1520_Plus_enu.pdf
DevOps_Doc/NAS/NAS技术文件/Syno_UsersGuide_NAServer_chs.pdf
DevOps_Doc/NAS/NAS操作记录文档/NAS-Lemu_Tech文件夹目录.txt
DevOps_Doc/NAS/NAS操作记录文档/NAS创建共享文件夹操作记录.md
DevOps_Doc/NAS/NAS操作记录文档/NAS新增存储空间操作.md
DevOps_Doc/NAS/NAS操作记录文档/NAS添加用户操作记录.md
DevOps_Doc/NAS/NAS操作记录文档/NAS添加用户群组.md
DevOps_Doc/NAS/NAS管理员文档/lsblk命令.md
DevOps_Doc/NAS/NAS管理员文档/NAS存储空间与用户帐号分配.md
DevOps_Doc/NAS/NAS管理员文档/NAS的安装与配置.md
DevOps_Doc/NAS/NAS管理员文档/NAS的需求.md
DevOps_Doc/NAS/NAS管理员文档/NAS管理员文档.md
DevOps_Doc/NAS/NAS红盘报价单.md
DevOps_Doc/NAS/NAS遇到的问题/NAS硬盘无法初始化.md
DevOps_Doc/NAS/README.md
```
> 您还可以将查找命令的各种开关组合在一起。例如，要查找目录中的所有文件，您可以使用：
```bash
find /path -type f -name '*.pdf'

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/NAS/ -type f -name '*.pdf'
DevOps_Doc/NAS/NAS学习文档笔记/lect04_FileSystem.pdf
DevOps_Doc/NAS/NAS技术文件/Syno_HIG_DS1520_Plus_enu.pdf
DevOps_Doc/NAS/NAS技术文件/Syno_UsersGuide_NAServer_chs.pdf
```
> 同样，要查找以"Get"开头的目录，请使用:
```bash
find /path -type d -name 'Get*'

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/Shell/ -type f -name 'Get*'
DevOps_Doc/Shell/Shell_Example/GetFiledate.py
DevOps_Doc/Shell/Shell_Example/GetFIleList.sh
DevOps_Doc/Shell/Shell_Example/GetInCommonUseOrder.sh
DevOps_Doc/Shell/Shell_Example/Getlogin.sh
DevOps_Doc/Shell/Shell_Example/Get_OS.sh
```
### 查找空文件
> 查找命令支持标记以搜索空文件和目录。空文件是其中没有内容的文件，而空目录是其中没有文件或子方向的文件。-empty

> 例如，如果您想在你要找的目录中查找列表空目录，您可以运行：
```bash
find ~ -type d -empty

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/c/Users/KiZai$ find ~ -type d -empty
/home/kizaiwsl2/.config/procps
/home/kizaiwsl2/.local/share/nano
```
### 否定匹配
> 查找命令还允许您"否定"匹配项。当我们有一个标准从我们的搜索中排除时，这是有用的。
> 例如，您希望在目录中查找没有扩展的文件。您可以在开关前面添加感叹号（）来否定开关，如下面例子所示,把不是“Get”开头的文件全部找出来：
```bash
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/Shell/ -type f ! -name 'Get*'
DevOps_Doc/Shell/.gitkeep
DevOps_Doc/Shell/20210728/100div3.sh
DevOps_Doc/Shell/20210728/99.sh
DevOps_Doc/Shell/20210728/countrows.sh
DevOps_Doc/Shell/20210728/sum1to100.sh
DevOps_Doc/Shell/20210728/sum1ton.sh
DevOps_Doc/Shell/20210728/test.txt
DevOps_Doc/Shell/20210728/yesno.sh
DevOps_Doc/Shell/example.sh
DevOps_Doc/Shell/mcd.sh
DevOps_Doc/Shell/script.py
DevOps_Doc/Shell/Shell_Example/CountRamUse.sh
DevOps_Doc/Shell/Shell_Example/CountUserNum.sh
DevOps_Doc/Shell/Shell_Example/cron定时脚本设计文档.md
DevOps_Doc/Shell/Shell_Example/Cron语法.md
DevOps_Doc/Shell/Shell_Example/df-sed-awk-du-grep.md
DevOps_Doc/Shell/Shell_Example/FindActiveIp.sh
DevOps_Doc/Shell/Shell_Example/find指令.md
DevOps_Doc/Shell/Shell_Example/fu.png
DevOps_Doc/Shell/Shell_Example/get_date.sh
DevOps_Doc/Shell/Shell_Example/JudgeSpeUserLogin.sh
DevOps_Doc/Shell/Shell_Example/JudgeUserLogin.sh
DevOps_Doc/Shell/Shell_Example/Lmadmin.csv
DevOps_Doc/Shell/Shell_Example/Lmdevops.csv
DevOps_Doc/Shell/Shell_Example/Lmtech.csv
DevOps_Doc/Shell/Shell_Example/mkdirdate.sh
DevOps_Doc/Shell/Shell_Example/MonCpuUse.sh
DevOps_Doc/Shell/Shell_Example/nastest.log
DevOps_Doc/Shell/Shell_Example/nastest.sh
DevOps_Doc/Shell/Shell_Example/NAS监视器脚本操作记录.md
DevOps_Doc/Shell/Shell_Example/NAS监视器设计文档.md
DevOps_Doc/Shell/Shell_Example/plot.py
DevOps_Doc/Shell/Shell_Example/Shell监控用户操作轨迹.md
DevOps_Doc/Shell/Shell_Example/Shell监视器设计文档.md
DevOps_Doc/Shell/Shell_Example/Shell脚本监控磁盘存储预警设计文档.md
DevOps_Doc/Shell/Shell_Example/Shell脚本监控磁盘存储预警设计文档Shell脚本监控磁盘存储预警操作文档.md
DevOps_Doc/Shell/Shell_Example/StaLogSize.sh
DevOps_Doc/Shell/Shell_Example/tempCodeRunnerFile.sh
DevOps_Doc/Shell/Shell_Notes.md
```
### 根据日期和时间搜索文件
>有时，您可能需要根据文件被访问或修改的时间来搜索文件。在 Linux 系统中，与文件相关的"时间"有三种类型：
>> * 修改时间：上次修改文件内容的时间。
>> * 访问时间：上次读取文件的时间。
>> * 更改时间：上次修改文件（其内容或元数据（如权限）时

> 要根据修改、访问或更改时间查找文件，查找命令具有和切换。这些开关允许您根据天数过滤文件。在这里，"一天"是指24小时。```-mtime-atime-ctime```

> 如何使用这些标志?
>> * -mtime 2： 文件在两天前（即在过去 48 到 72 小时内）进行了修改。
>> * -mtime -2： 文件在不到两天前（即在过去 48 小时内）被修改。
>> * -mtime +2： 文件在 2 天以上（即 3 天或更长时间，即 72 小时或更长时间）被修改。

> 因此，要查找两天前修改的文件，可以使用:
```bash
find /usr -type f -mtime 2

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/Shell/Shell_Example/ -type f -mtime 2
DevOps_Doc/Shell/Shell_Example/Cron语法.md
DevOps_Doc/Shell/Shell_Example/df-sed-awk-du-grep.md
DevOps_Doc/Shell/Shell_Example/Get_OS.sh
DevOps_Doc/Shell/Shell_Example/NAS监视器设计文档.md
```
> 如果你发现一天有点太长，你也可以使用，或过滤几分钟。因此，意味着 5 分钟前访问的文件，意味着文件在不到 5 分钟前被修改，等等。```-mmin-amin-cmin-amin +5-amin -5```
> 您可以同时使用其中的许多标志。如果您想查找 10 天前修改并在 1 分钟前访问的文件，请运行：
```bash
find /usr -type f -mtime +10 -amin 1

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/Shell/Shell_Example/ -type f -mtime +10 -amin 1
DevOps_Doc/Shell/Shell_Example/Shell监视器设计文档.md
```
### 根据大小进行搜索
> 查找命令具有一个开关，允许用户根据其大小查找文件。由于只有文件可以具有文件大小，当您使用此交换机时，不会列出任何目录。-size

> 为了指定文件大小，我们放置了一个数字，然后放了一封信来表示该单位。您可以使用以下字母：
>> * c： 字节
>> * k： 千字节
>> * M： 兆字节
>> * G： 千兆字节

> 此外，您可以使用或强加"大于"或"小于"条件。+-

> 让我们用几个例子来理解这一点。如果您想列出系统中大小为小于 10 KB 的所有文件，请使用：
```bash
find /path -size -10k

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/Shell/Shell_Example/ -size -10k
DevOps_Doc/Shell/Shell_Example/
DevOps_Doc/Shell/Shell_Example/CountRamUse.sh
DevOps_Doc/Shell/Shell_Example/CountUserNum.sh
DevOps_Doc/Shell/Shell_Example/cron定时脚本设计文档.md
DevOps_Doc/Shell/Shell_Example/Cron语法.md
DevOps_Doc/Shell/Shell_Example/FindActiveIp.sh
DevOps_Doc/Shell/Shell_Example/find指令.md
DevOps_Doc/Shell/Shell_Example/GetFiledate.py
DevOps_Doc/Shell/Shell_Example/GetFIleList.sh
DevOps_Doc/Shell/Shell_Example/GetInCommonUseOrder.sh
DevOps_Doc/Shell/Shell_Example/Getlogin.sh
DevOps_Doc/Shell/Shell_Example/get_date.sh
DevOps_Doc/Shell/Shell_Example/Get_OS.sh
DevOps_Doc/Shell/Shell_Example/JudgeSpeUserLogin.sh
DevOps_Doc/Shell/Shell_Example/JudgeUserLogin.sh
DevOps_Doc/Shell/Shell_Example/Lmadmin.csv
DevOps_Doc/Shell/Shell_Example/Lmdevops.csv
DevOps_Doc/Shell/Shell_Example/Lmtech.csv
DevOps_Doc/Shell/Shell_Example/mkdirdate.sh
DevOps_Doc/Shell/Shell_Example/MonCpuUse.sh
DevOps_Doc/Shell/Shell_Example/nastest.log
DevOps_Doc/Shell/Shell_Example/nastest.sh
DevOps_Doc/Shell/Shell_Example/NAS监视器脚本操作记录.md
DevOps_Doc/Shell/Shell_Example/NAS监视器设计文档.md
DevOps_Doc/Shell/Shell_Example/plot.py
DevOps_Doc/Shell/Shell_Example/Shell监控用户操作轨迹.md
DevOps_Doc/Shell/Shell_Example/Shell监视器设计文档.md
DevOps_Doc/Shell/Shell_Example/Shell脚本监控磁盘存储预警设计文档.md
DevOps_Doc/Shell/Shell_Example/Shell脚本监控磁盘存储预警设计文档Shell脚本监控磁盘存储预警操作文档.md
DevOps_Doc/Shell/Shell_Example/StaLogSize.sh
DevOps_Doc/Shell/Shell_Example/tempCodeRunnerFile.sh
```
> 同样，要查找大小大于 1MB 的文件，请使用：
```bash
find / -size +1M

# 例如：
kizaiwsl2@KIZAI-DESKTOP:/mnt/f/GitLab-Runner/company-tech-doc$ find DevOps_Doc/NAS/ -size +1M
DevOps_Doc/NAS/NAS技术文件/Syno_HIG_DS1520_Plus_enu.pdf
DevOps_Doc/NAS/NAS技术文件/Syno_UsersGuide_NAServer_chs.pdf
```
### 基于权限进行搜索
> 查找命令支持该开关，以便根据权限进行搜索。它允许您基于数字和符号模式进行搜索。如果您不熟悉权限，您可以在[此处](https://www.booleanworld.com/introduction-linux-file-permissions/)找到概述。```-perm```

