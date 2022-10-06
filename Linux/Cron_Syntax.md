# Cron语法

### 1. 语法指令
```bash
crontab [-u user] {-l|-e|-r|-i}
```q
#### crontab的语法格式：
```bash
分 时 日 月 星期 要运行的命令
```
#### crontab的条目举例：
1. 下面的例子表示每晚的21:30运行/apps/bin目录下的cleanup.sh。
```bash
30 21 * * * /apps/bin/cleanup.sh
```
2. 下面的例子表示每月1、10、22日的4:45运行/apps/bin目录下的backup.sh。
```bash
45 4 1,10,22 * * /apps/bin/backup.sh
```
3. 下面的例子表示每周六、周日的1:10运行一个find命令。
```bash
10 1 * * 6,0 /bin/find -name "core" -exec rm {} \;
```
4. 下面的例子表示在每天18:00至23:00之间每隔30分钟运行/ apps/bin目录下的dbcheck.sh。
```bash
0,30 18-23 * * * /apps/bin/dbcheck.sh
```
5. 下面的例子表示每星期六的11:00pm运行/apps/bin目录下的qtrend.sh。
```bash
0 23 * * 6 /apps/bin/qtrend.sh
``` 

 查看帮助
```bash
lemu-devops@lemudevops:/$ crontab --help
crontab: invalid option -- '-'
crontab: usage error: unrecognized option
usage:	crontab [-u user] file
	crontab [ -u user ] [ -i ] { -e | -l | -r }
		(default operation is replace, per 1003.2)
	-e	(edit user's crontab)
	-l	(list user's crontab)
	-r	(delete user's crontab)
	-i	(prompt before deleting user's crontab)

```
### 2. 指令说明
通过crontab我们可以在固定的间隔时间执行指定的系统指令或script脚本。时间间隔的单位是分、时、日、月、周及以上的任意组合。
### 3. 使用者权限文件
|文件 |说明
|:---|--
/etc/cron.deny |#该文件中所列用户不允许使用crontab命令
/etc/cron.allow |#该文件中所列用户允许使用crontab命令，优先于/etc/cron.deny
/etc/spool/cron/ |#所有用户crontab配置文件默认都存在此目录，文件名以用户名命令

注：
- crontab -e = vi /var/spool/cron/root
- crontab -l = cat /var/spool/cron/root

### 4、指令选项说明含义表
* -e (edit user's crontab) #编辑crontab文件内容
* -l (list user's crontab) #查看crontab文件内容
* -r (delete user's crontab) #删除crontab文件内容
* -i (prompt before deleting user's crontab) #删除crontab文件内容，删除前会提示。
* -u #指定使用的用户执行任务

### 5、指令的使用格式
#### 5.1.crond语法格式中时间段的含义:
```bash
lemu-devops@lemudevops:/$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#

```
#### 5.2. crontab语法格式中特殊符号含义
```
1. * // *号，表示任意时间，如每分、每小时、每日、每月、每周

2. - // 减号，表示分隔符，表示一个范围区间

3. ， // 逗号，表示分割时间段的意思。

4. /n // n代表数字，即“每隔n单位时间”

```

### 6、at命令
* at命令允许用户向cron守护进程提交作业，使其在稍后的时间运行。这里稍后的时间可能是指10min以后，也可能是指几天以后。如果你希望在一个月或更长的时间以后运行，最好还是使用crontab文件。
* 一旦一个作业被提交，at命令将会保留所有当前的环境变量，包括路径，不象crontab，只提供缺省的环境。该作业的所有输出都将以电子邮件的形式发送给用户，除非你对其输出进行了重定向，绝大多数情况下是重定向到某个文件中。
和crontab一样，根用户可以通过/etc目录下的at.allow和at.deny文件来控制哪些用户可以使用at命令，哪些用户不行。不过一般来说，对at命令的使用不如对crontab的使用限制那么严格。
#### 6.1. at命令的基本形式
```bash
at [-f  script] [-m -l -r] [time] [date]
```
参数说明：
* -f script 是所要提交的脚本或命令。
* -l 列出当前所有等待运行的作业。a t q命令具有相同的作用。
* -r 清除作业。为了清除某个作业，还要提供相应的作业标识（ID）；有些UNIX变体只接受atrm作为清除命令。
* -m 作业完成后给用户发邮件。
* time at命令的时间格式非常灵活；可以是H、HH.HHMM、HH:MM或H:M，其中H和M分别是小时和分钟。还可以使用a.m .或p.m .。
* date 日期格式可以是月份数或日期数，而且at命令还能够识别诸如today、tomorrow这样的词。
现在就让我们来看看如何提交作业。

#### 6.2. 使用at命令提交命令或脚本
* 使用at命令提交作业有几种不同的形式，可以通过命令行方式，也可以使用at命令提示符。一般来说在提交若干行的系统命令时，我使用at命令提示符方式，而在提交shell脚本时，使用命令行方式。
* 如果你想提交若干行的命令，可以在at命令后面跟上日期/时间并回车。然后就进入了at命令提示符，这时只需逐条输入相应的命令，然后按‘<CTRL-D>’退出。下面给出一个例子：
```bash
lemu-devops@lemudevops:~$ at 10:50
warning: commands will be executed using /bin/sh
at> fine /.csv "date" -print                 
at> <EOT>
job 1 at Sat Aug 14 10:50:00 2021
```
* 其中，<EOT>就是<CTRL-D>。在10:50系统将执行一个简单的find命令。你应当已经注意到，我所提交的作业被分配了一个唯一标识job 1。
* 下面这些日期/时间格式都是at命令可以接受的：
```bash
at 6.45am May12
at 11.10pm
at now + 1 hour
at 9am tomorrow
at now + 10 minutes - this time  spectification is my own favourite
```
* 如果希望向at命令提交一个shell脚本，使用其命令行方式即可。在提交脚本时使用-f选项。
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ at 12:54 -f Get_OS.sh 
warning: commands will be executed using /bin/sh
job 2 at Fri Aug 13 12:54:00 2021

```
### 6.3. 清除一个作业：
命令格式：
```bash
atrm [jod no] or at -r[jod no]
```
* 要清除某个作业，首先要执行at-l命令，以获取相应的作业标识，然后对该作业标识使用at -r 命令，清除该作业。有些系统使用at-r [job no]命令清除作业。
```bash
# 列出作业列表：
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ at -l
1	Sat Aug 14 10:50:00 2021 a lemu-devops

# 删除作业1
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ at -r 1
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ at -l
# 发现作业已经被删除了。
```