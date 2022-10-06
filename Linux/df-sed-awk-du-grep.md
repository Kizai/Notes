# df-sed-awk-du-grep学习笔记

## 参考来源：
- https://www.runoob.com/linux/
- https://www.runoob.com/linux/linux-comm-awk.html

## df命令：
 df（英文全拼：disk free） 命令用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计。
### 语法：
```bash
df [选项]... [FILE]...
```
- 文件-a, --all 包含所有的具有 0 Blocks 的文件系统
- 文件--block-size={SIZE} 使用 {SIZE} 大小的 Blocks
- 文件-h, --human-readable 使用人类可读的格式(预设值是不加这个选项的...)
- 文件-H, --si 很像 -h, 但是用 1000 为单位而不是用 1024
- 文件-i, --inodes 列出 inode 资讯，不列出已使用 block
- 文件-k, --kilobytes 就像是 --block-size=1024
- 文件-l, --local 限制列出的文件结构
- 文件-m, --megabytes 就像 --block-size=1048576
- 文件--no-sync 取得资讯前不 sync (预设值)
- 文件-P, --portability 使用 POSIX 输出格式
- 文件--sync 在取得资讯前 sync
- 文件-t, --type=TYPE 限制列出文件系统的 TYPE
- 文件-T, --print-type 显示文件系统的形式
- 文件-x, --exclude-type=TYPE 限制列出文件系统不要显示 TYPE
- 文件-v (忽略)
- 文件--help 显示这个帮手并且离开
- 文件--version 输出版本资讯并且离开
### 实例：
```df```显示文件系统的磁盘使用情况统计：默认单位是byte
```bash
kizaiwsl2@KIZAI-DESKTOP:~$ df
Filesystem     1K-blocks      Used Available Use% Mounted on
/dev/sdb       263174212   1465208 248270848   1% /
tmpfs            9706744         0   9706744   0% /mnt/wsl
tools          499207124 377409084 121798040  76% /init
none             9704660         0   9704660   0% /dev
none             9706744         4   9706740   1% /run
none             9706744         0   9706744   0% /run/lock
none             9706744         0   9706744   0% /run/shm
none             9706744         0   9706744   0% /run/user
tmpfs            9706744         0   9706744   0% /sys/fs/cgroup
C:\            499207124 377409084 121798040  76% /mnt/c
D:\            409599996 303194164 106405832  75% /mnt/d
F:\            567158780 356728696 210430084  63% /mnt/f
```
使用```df filename```指定文件夹显示磁盘使用信息：
```bash
kizaiwsl2@KIZAI-DESKTOP:/home$ ls
kizaiwsl2  script  test
kizaiwsl2@KIZAI-DESKTOP:/home$ df test/
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sdb       263174212 1465208 248270848   1% /
kizaiwsl2@KIZAI-DESKTOP:/home$
```
-h选项，通过它可以产生人类可读的格式df命令的输出：
```bash
kizaiwsl2@KIZAI-DESKTOP:/home$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb        251G  1.4G  237G   1% /
tmpfs           9.3G     0  9.3G   0% /mnt/wsl
tools           477G  360G  117G  76% /init
none            9.3G     0  9.3G   0% /dev
none            9.3G  4.0K  9.3G   1% /run
none            9.3G     0  9.3G   0% /run/lock
none            9.3G     0  9.3G   0% /run/shm
none            9.3G     0  9.3G   0% /run/user
tmpfs           9.3G     0  9.3G   0% /sys/fs/cgroup
C:\             477G  360G  117G  76% /mnt/c
D:\             391G  290G  102G  75% /mnt/d
F:\             541G  341G  201G  63% /mnt/f
```
## sed命令：
- sed 命令是利用脚本来处理文本文件。
- sed 可依照脚本的指令来处理、编辑文本文件。
- sed 主要用来自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序等。
### 语法：
```bash
sed [-hnV][-e<script>][-f<script文件>][文本文件]
```
#### 参数说明：

* -e<脚本>或--expression=<脚本> 以选项中指定的script来处理输入的文本文件。
* -f<脚本>或--file=<脚本> 以选项中指定的script文件来处理输入的文本文件。
* -h或--help 显示帮助。
* -n或--quiet或--silent 仅显示script处理后的结果。
* -V或--version 显示版本信息。
#### 动作说明：

* a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
* c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
* d ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；
* i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
* p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
* s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！

### 实例：
使用```vim test```新建一个test文件进行测试,输入```Hello Linux```
再使用```cat test```查看内容
```bash
kizaiwsl2@KIZAI-DESKTOP:/home/test$ cat test
Hello Linux
```
在test文件的第一行后添加一行，并将结果输出到标准输出，在命令行提示符下输入如下命令：
```bash
sed -e 1a\newLine test

kizaiwsl2@KIZAI-DESKTOP:/home/test$ sed -e 1a\newline test
Hello Linux
newline
``` 
####  数据的搜寻并显示
例如：显示磁盘名称包含字母d的信息并使用-h人类可阅读方式打印出来：
```bash
kizaiwsl2@KIZAI-DESKTOP:/home/test$ df -h | sed -n '/d/p'
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb        251G  1.4G  237G   1% /
none            9.3G     0  9.3G   0% /dev
D:\             391G  290G  102G  75% /mnt/d
```

## awk命令：
- AWK 是一种处理文本文件的语言，是一个强大的文本分析工具。
- 之所以叫 AWK 是因为其取了三位创始人 Alfred Aho，Peter Weinberger, 和 Brian Kernighan 的 Family Name 的首字符。
### 语法：
```bash
awk [选项参数] 'script' var=value file(s)
或
awk [选项参数] -f scriptfile var=value file(s)
```
#### 选项参数说明：

| 命令                                                    | 效果                                                                                                                                                     |
|:--------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -F fs or --field-separator fs                           | 指定输入文件折分隔符，fs是一个字符串或者是一个正则表达式，如-F:。                                                                                        |
| -v var=value or --asign var=value                       | 赋值一个用户定义变量。                                                                                                                                   |
| -f scripfile or --file scriptfile                       | 从脚本文件中读取awk命令。                                                                                                                                |
| -mf nnn and -mr nnn                                     | 对nnn值设置内在限制，-mf选项限制分配给nnn的最大块数目；-mr选项限制记录的最大数目。这两个功能是Bell实验室版awk的扩展功能，在标准awk中不适用。             |
| -W compact or --compat, -W traditional or --traditional | 在兼容模式下运行awk。所以gawk的行为和标准的awk完全一样，所有的awk扩展都被忽略。                                                                          |
| -W copyleft or --copyleft, -W copyright or --copyright  | 打印简短的版权信息。                                                                                                                                     |
| -W help or --help, -W usage or --usage                  | 打印全部awk选项和每个选项的简短说明。                                                                                                                    |
| -W lint or --lint                                       | 打印不能向传统unix平台移植的结构的警告。                                                                                                                 |
| -W lint-old or --lint-old                               | 打印关于不能向传统unix平台移植的结构的警告。                                                                                                             |
| -W posix                                                | 打开兼容模式。但有以下限制，不识别：/x、函数关键字、func、换码序列以及当fs是一个空格时，将新行作为一个域分隔符；操作符**和**=不能代替^和^=；fflush无效。 |
| -W re-interval or --re-inerval                          | 允许间隔正则表达式的使用，参考(grep中的Posix字符类)，如括号表达式[[:alpha:]]。                                                                           |
| -W source program-text or --source program-text         | 使用program-text作为源代码，可与-f命令混用。                                                                                                             |
| -W version or --version                                 | 打印bug报告信息的版本。                                                                                                                                  |
### 运算符
| 运算符                  | 描述                             |
|:------------------------:|:---------------------------------|
| = += -= *= /= %= ^= **= | 赋值                             |
| ?:                      | C条件表达式                      |
| &&                      | 逻辑与                           |
| ~ 和 !~                 | 匹配正则表达式和不匹配正则表达式 |
| < <= > >= != ==         | 关系运算符                       |
| 空格                    | 连接                             |
| + -                     | 加，减                           |
| * / %                   | 乘，除与求余                     |
| + - !                   | 一元加，减和逻辑非               |
| ^ ***                   | 求幂                             |
| ++ --                   | 增加或减少，作为前缀或后缀       |
| $                       | 字段引用                         |
| in                      | 数组成员                         |
### 内建变量
| 变量        | 描述                                              |
|:------------|:--------------------------------------------------|
| $n          | 当前记录的第n个字段，字段间由FS分隔               |
| $0          | 完整的输入记录                                    |
| ARGC        | 命令行参数的数目                                  |
| ARGIND      | 命令行中当前文件的位置(从0开始算)                 |
| ARGV        | 包含命令行参数的数组                              |
| CONVFMT     | 数字转换格式(默认值为%.6g)ENVIRON环境变量关联数组 |
| ERRNO       | 最后一个系统错误的描述                            |
| FIELDWIDTHS | 字段宽度列表(用空格键分隔)                        |
| FILENAME    | 当前文件名                                        |
| FNR         | 各文件分别计数的行号                              |
| FS          | 字段分隔符(默认是任何空格)                        |
| IGNORECASE  | 如果为真，则进行忽略大小写的匹配                  |
| NF          | 一条记录的字段的数目                              |
| NR          | 已经读出的记录数，就是行号，从1开始               |
| OFMT        | 数字的输出格式(默认值是%.6g)                      |
| OFS         | 输出字段分隔符，默认值与输入字段分隔符一致。      |
| ORS         | 输出记录分隔符(默认值是一个换行符)                |
| RLENGTH     | 由match函数所匹配的字符串的长度                   |
| RS          | 记录分隔符(默认是一个换行符)                      |
| RSTART      | 由match函数所匹配的字符串的第一个位置             |
| SUBSEP      | 数组下标分隔符(默认值是/034)                      |
### 实例：
#### 打印文件Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example/test.csv中所有的行，不指定任何模式：
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ awk '//{print}' test.csv 
created_date,filesname,size,used,avail,file_number
2021-08-10,Lemu_Tech,5000,100,4900,600
2021-08-10,Lemu_Admin,2000,100,1900,50
2021-08-10，Lemu_DevOps,1700,100,1600,20
2021-08-11,Lemu_Tech,5000,150,4850,700 
2021-08-11,Lemu_Admin,2000,140,1860,100
2021-08-11,Lemu_DevOps,1700,130,1570,50
2021-08-12,Lemu_Tech,5000,300,4700,1000
2021-08-12,Lemu_Admin,2000,200,1800,200
2021-08-12,Lemu_DevOps,1700,250,1450,100
2021-08-13,Lemu_Tech,5000,500,4500,1500
2021-08-13,Lemu_Admin,2000,300,1700,300 
2021-08-13,Lemu_DevOps,1700,500,1200,300

```
#### 指定匹配```Lemu_Tech```模式，因此awk匹配文件test.csv中的```Lemu_Tech```的那些行：
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ awk '/Lemu_Tech/{print}' test.csv 
2021-08-10,Lemu_Tech,5000,100,4900,600
2021-08-11,Lemu_Tech,5000,150,4850,700 
2021-08-12,Lemu_Tech,5000,300,4700,1000
2021-08-13,Lemu_Tech,5000,500,4500,1500

```
#### 使用通配符```(.)```,我这里用它来匹配包含：A、Ad、Admin的字符串：
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ awk '/.Ad/{print}' test.csv 
2021-08-10,Lemu_Admin,2000,100,1900,50
2021-08-11,Lemu_Admin,2000,140,1860,100
2021-08-12,Lemu_Admin,2000,200,1800,200
2021-08-13,Lemu_Admin,2000,300,1700,300 
```
#### 结合结合集合 [ character(s) ] 使用 awk进行匹配，下面打印含有以```l```、```d```、```D```、```o```字符的数据：
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ awk '/[ldDo]/{print}' test.csv
created_date,filesname,size,used,avail,file_number
2021-08-10,Lemu_Admin,2000,100,1900,50
2021-08-10，Lemu_DevOps,1700,100,1600,20
2021-08-11,Lemu_Admin,2000,140,1860,100
2021-08-11,Lemu_DevOps,1700,130,1570,50
2021-08-12,Lemu_Admin,2000,200,1800,200
2021-08-12,Lemu_DevOps,1700,250,1450,100
2021-08-13,Lemu_Admin,2000,300,1700,300
2021-08-13,Lemu_DevOps,1700,500,1200,300
```
#### 计算文件大小：
```bash
# 计算该目录下.csv文件的大小
lemu-devops@lemudevops:~/GitLab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ ls -l *.csv | awk '{sum+=$5} END {print sum}'
549

# 计算该目录下.sh文件的大小
lemu-devops@lemudevops:~/GitLab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ ls -l *.sh | awk '{sum+=$5} END {print sum}'
3152
```
## 正则表达式：
- 正则表达式(regular expression)描述了一种字符串匹配的模式（pattern），可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等。
- 正则表达式是由普通字符（例如字符 a 到 z）以及特殊字符（称为"元字符"）组成的文字模式。模式描述在搜索文本时要匹配的一个或多个字符串。正则表达式作为一个模板，将某个字符模式与所搜索的字符串进行匹配。
### 普通字符：
| 字符   | 描述                                                                                                             |
|:-------|:-----------------------------------------------------------------------------------------------------------------|
| [ABC]  | 匹配 [...] 中的所有字符，例如 [aeiou] 匹配字符串 "google runoob taobao" 中所有的 e o u a 字母。                  |
| [^ABC] | 匹配除了 [...] 中字符的所有字符，例如 [^aeiou] 匹配字符串 "google runoob taobao" 中除了 e o u a 字母的所有字母。 |
| [A-Z]  | [A-Z] 表示一个区间，匹配所有大写字母，[a-z] 表示所有小写字母。                                                   |
| .      | 匹配除换行符（\n、\r）之外的任何单个字符，相等于 [^\n\r]。                                                       |
| [\s\S] | 匹配所有。\s 是匹配所有空白符，包括换行，\S 非空白符，不包括换行。                                               |
| \w     | 匹配字母、数字、下划线。等价于 [A-Za-z0-9_]                                                                      |

### 非打印字符：
| 字符 | 描述                                                                                                                              |
|:-----|:----------------------------------------------------------------------------------------------------------------------------------|
| \cx  | 匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。 |
| \f   | 匹配一个换页符。等价于 \x0c 和 \cL。                                                                                              |
| \n   | 匹配一个换行符。等价于 \x0a 和 \cJ。                                                                                              |
| \r   | 匹配一个回车符。等价于 \x0d 和 \cM。                                                                                              |
| \s   | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意 Unicode 正则表达式会匹配全角空格符。                   |
| \S   | 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。                                                                                       |
| \t   | 匹配一个制表符。等价于 \x09 和 \cI。                                                                                              |
| \v   | 匹配一个垂直制表符。等价于 \x0b 和 \cK。                                                                                          |

### 特殊字符

| 特别字符 | 描述                                                                                                                                                     |
|:---------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| $        | 匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用 \$。                              |
| ( )      | 标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。要匹配这些字符，请使用 \( 和 \)。                                                          |
| *        | 匹配前面的子表达式零次或多次。要匹配 * 字符，请使用 \*。                                                                                                 |
| +        | 匹配前面的子表达式一次或多次。要匹配 + 字符，请使用 \+。                                                                                                 |
| .        | 匹配除换行符 \n 之外的任何单字符。要匹配 . ，请使用 \. 。                                                                                                |
| [        | 标记一个中括号表达式的开始。要匹配 [，请使用 \[。                                                                                                        |
| ?        | 匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 ? 字符，请使用 \?。                                                                         |
| \        | 将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\\' 匹配 "\"，而 '\(' 则匹配 "("。 |
| ^        | 匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使用 \^。 |
| {        | 标记限定符表达式的开始。要匹配 {，请使用 \{。                                                                                                            |
|          |                                                                                                                                                          |

### 限定符
| 字符  | 描述                                                                                                                                                                     |
|:------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *     | 匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。                                                                                            |
| +     | 匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。                                                                        |
| ?     | 匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 、 "does" 中的 "does" 、 "doxy" 中的 "do" 。? 等价于 {0,1}。                                                 |
| {n}   | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。                                                                  |
| {n,}  | n 是一个非负整数。至少匹配n 次。例如，'o{2,}' 不能匹配 "Bob" 中的 'o'，但能匹配 "foooood" 中的所有 o。'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。                       |
| {n,m} | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。例如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 'o?'。请注意在逗号和两个数之间不能有空格。 |

### 定位符：
| 字符 | 描述                                                                                                  |
|:-----|:------------------------------------------------------------------------------------------------------|
| ^    | 匹配输入字符串开始的位置。如果设置了 RegExp 对象的 Multiline 属性，^ 还会与 \n 或 \r 之后的位置匹配。 |
| $    | 匹配输入字符串结尾的位置。如果设置了 RegExp 对象的 Multiline 属性，$ 还会与 \n 或 \r 之前的位置匹配。 |
| \b   | 匹配一个单词边界，即字与空格间的位置。                                                                |
| \B   | 非单词边界匹配。                                                                                      |

## du命令
* Linux du （英文全拼：disk usage）命令用于显示目录或文件的大小。
* du 会显示指定的目录或文件所占用的磁盘空间。

### 语法：
```bash
du [-abcDhHklmsSx][-L <符号连接>][-X <文件>][--block-size][--exclude=<目录或文件>][--max-depth=<目录层数>][--help][--version][目录或文件]
```
#### 参数说明：

| 参数                                  | 功能                                                               |
|:--------------------------------------|--------------------------------------------------------------------|
| -a或-all                              | 显示目录中个别文件的大小。                                         |
| -b或-bytes                            | 显示目录或文件大小时，以byte为单位。                               |
| -c或--total                           | 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。   |
| -D或--dereference-args                | 显示指定符号连接的源文件大小。                                     |
| -h或--human-readable                  | 以K，M，G为单位，提高信息的可读性。                                |
| -H或--si                              | 与-h参数相同，但是K，M，G是以1000为换算单位。                      |
| -k或--kilobytes                       | 以1024 bytes为单位。                                               |
| -l或--count-links                     | 重复计算硬件连接的文件。                                           |
| -L<符号连接>或--dereference<符号连接> | 显示选项中所指定符号连接的源文件大小。                             |
| -m或--megabytes                       | 以1MB为单位。                                                      |
| -s或--summarize                       | 仅显示总计。                                                       |
| -S或--separate-dirs                   | 显示个别目录的大小时，并不含其子目录的大小。                       |
| -x或--one-file-xystem                 | 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。 |
| -X<文件>或--exclude-from=<文件>       | 在<文件>指定目录或文件。                                           |
| --exclude=<目录或文件>                | 略过指定的目录或文件。                                             |
| --max-depth=<目录层数>                | 超过指定层数的目录后，予以忽略。                                   |
| --help                                | 显示帮助。                                                         |
| --version                             | 显示版本信息。                                                     |
### 实例：
#### 显示DevOps文件夹和文件的所占空间，使用人类可读性的方式：
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc$ du -h
14M	./NAS/NAS技术文件
292K	./NAS/NAS学习文档笔记
24K	./NAS/NAS操作记录文档
12K	./NAS/NAS遇到的问题
56K	./NAS/NAS管理员文档
14M	./NAS
32K	./Shell/20210728
124K	./Shell/Shell_Example
180K	./Shell
14M	.

```
#### 显示指定文件的大小：
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc$ du -h 每日工作计划.md
12K	每日工作计划.md
```

## grep命令：
- Linux grep 命令用于查找文件里符合条件的字符串。
- grep 指令用于查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。若不指定任何文件名称，或是所给予的文件名为 -，则 grep 指令会从标准输入设备读取数据。

### 语法：
```bash
grep [-abcEFGhHilLnqrsvVwxy][-A<显示行数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]
```
#### 参数说明

| 参数| 功能                                                                                                                                  |
| :------------------------------|----------------------------------------------------------------------------------------------------------- |
| -a 或 --text | 不要忽略二进制的数据。                                                                                                      |
| -A<显示行数> 或 --after-context=<显示行数> | 除了显示符合范本样式的那一列之外，并显示该行之后的内容。                                      |
| -b 或 --byte-offset | 在显示符合样式的那一行之前，标示出该行第一个字符的编号。                                                             |
| -B<显示行数> 或 --before-context=<显示行数> | 除了显示符合样式的那一行之外，并显示该行之前的内容。                                         |
| -c 或 --count | 计算符合样式的列数。                                                                                                       |
| -C<显示行数> 或 --context=<显示行数>或-<显示行数> | 除了显示符合样式的那一行之外，并显示该行之前后的内容。                                 |
| -d <动作> 或 --directories=<动作> | 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。                   |
| -e<范本样式> 或 --regexp=<范本样式> | 指定字符串做为查找文件内容的样式。                                                                   |
| -E 或 --extended-regexp | 将样式为延伸的正则表达式来使用。                                                                                 |
| -f<规则文件> 或 --file=<规则文件> | 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。 |
| -F 或 --fixed-regexp | 将样式视为固定字符串的列表。                                                                                        |
| -G 或 --basic-regexp | 将样式视为普通的表示法来使用。                                                                                      |
| -h 或 --no-filename | 在显示符合样式的那一行之前，不标示该行所属的文件名称。                                                               |
| -H 或 --with-filename | 在显示符合样式的那一行之前，表示该行所属的文件名称。                                                               |
| -i 或 --ignore-case | 忽略字符大小写的差别。                                                                                               |
| -l 或 --file-with-matches | 列出文件内容符合指定的样式的文件名称。                                                                         |
| -L 或 --files-without-match | 列出文件内容不符合指定的样式的文件名称。                                                                     |
| -n 或 --line-number | 在显示符合样式的那一行之前，标示出该行的列数编号。                                                                   |
| -o 或 --only-matching | 只显示匹配PATTERN 部分。                                                                                           |
| -q 或 --quiet或--silent | 不显示任何信息。                                                                                                 |
| -r 或 --recursive | 此参数的效果和指定"-d recurse"参数相同。                                                                               |
| -s 或 --no-messages | 不显示错误信息。                                                                                                     |
| -v 或 --invert-match | 显示不包含匹配文本的所有行。                                                                                        |
| -V 或 --version | 显示版本信息。                                                                                                           |
| -w 或 --word-regexp | 只显示全字符合的列。                                                                                                 |
| -x --line-regexp | 只显示全列符合的列。                                                                                                    |
| -y | 此参数的效果和指定"-i"参数相同。                                                                                                      |
### 实例
#### 在目录文件下查找后缀```csv```字样的文件包含```date```字符串字符串。
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ grep date *csv
lmadmin.csv:created_date,filesname,size,used,avail,file_number
lmdevops.csv:created_date,filesname,size,used,avail,file_number
lmtechtest.csv:created_date,filesname,size,used,avail,file_number
```
#### 以递归方式查找符合条件的文件。查找指定目录下所有文件中包含```used```的文件，并打印该字符串所在行的内容。
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ grep -r date
GetFIleList.sh:d=`date -d "-5 min" +%Y%m%d%H%M`
Shell监控用户操作轨迹.md:        exec /usr/bin/script -t 2>/var/log/script/$USER-$UID-`date +%Y%m%d%H%M`.date -a -f -q /var/log/script/$USER-$UID-`date +%Y%m%d%H%M`.log
Shell监控用户操作轨迹.md:-rw-rw-r--  1 lemu001 lemu001   12 7月  31 20:00 lemu001-1000-202107311949.date
Shell监控用户操作轨迹.md:-rw-rw-r--  1 lemu001 lemu001  229 7月  31 20:06 lemu001-1000-202107311950.date
Shell监控用户操作轨迹.md:lemu001@lemu-devops:/var/log/script$ scriptreplay lemu001-1000-202107311950.date  lemu001-1000-202107311950.log 
cron定时脚本设计文档.md:# print system date 1min
cron定时脚本设计文档.md:* * * * * echo “$(date)“ >> /var/log/cronlog/cronsysdate.log
cron定时脚本设计文档.md:# print system date 1min
cron定时脚本设计文档.md:* * * * * echo “$(date)” >> /var/log/cronlog/cronsysdate.log 
cron定时脚本设计文档.md:lemu-devops@lemudevops:/$ tail -f cronsysdate.log
lmadmin.csv:created_date,filesname,size,used,avail,file_number
NAS监视器设计文档.md:created_date,filesname,size,used,avail,file_number
NAS监视器设计文档.md:created_date,filesname,size,used,avail,file_number
NAS监视器设计文档.md:created_date,filesname,size,used,avail,file_number
StaLogSize.sh:t=`date +%H`
StaLogSize.sh:d=`date +%F-%H`
df-sed-awk-du-grep.md:created_date,filesname,size,used,avail,file_number
df-sed-awk-du-grep.md:created_date,filesname,size,used,avail,file_number
df-sed-awk-du-grep.md:#### 在目录文件下查找后缀```csv```字样的文件包含```date```字符串字符串。
df-sed-awk-du-grep.md:lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ grep date *csv
df-sed-awk-du-grep.md:lmadmin.csv:created_date,filesname,size,used,avail,file_number
df-sed-awk-du-grep.md:lmdevops.csv:created_date,filesname,size,used,avail,file_number
df-sed-awk-du-grep.md:lmtechtest.csv:created_date,filesname,size,used,avail,file_number
mkdirdate.sh: if [ ! -d "$(pwd)/$(date +%Y-%m-%d)"]; then
mkdirdate.sh: touch $(pwd)/$(date +%Y-%m-%d)
lmtechtest.csv:created_date,filesname,size,used,avail,file_number
lmdevops.csv:created_date,filesname,size,used,avail,file_number
```
#### 反向查找。前面各个例子是查找并打印出符合条件的行，通过"-v"参数可以打印出不符合条件行的内容。现在查找文件名中包含```csv```的文件不包含```date```的行：
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ grep -v date *csv
lmadmin.csv:2021-08-10,Lemu_Admin,2000,100,1900,50
lmadmin.csv:2021-08-11,Lemu_Admin,2000,140,1860,100
lmadmin.csv:2021-08-12,Lemu_Admin,2000,200,1800,200
lmadmin.csv:2021-08-13,Lemu_Admin,2000,300,1700,300 
lmdevops.csv:2021-08-10,Lemu_DevOps,1700,100,1600,20
lmdevops.csv:2021-08-11,Lemu_DevOps,1700,130,1570,50
lmdevops.csv:2021-08-12,Lemu_DevOps,1700,250,1450,100 
lmdevops.csv:2021-08-13,Lemu_DevOps,1700,500,1200,300
lmtechtest.csv:2021-08-10,Lemu_Tech,5000,100,4900,600
lmtechtest.csv:2021-08-11,Lemu_Tech,5000,150,4850,700 
lmtechtest.csv:2021-08-12,Lemu_Tech,5000,300,4700,1000
lmtechtest.csv:2021-08-13,Lemu_Tech,5000,500,4500,1500

```
#### 使用正则表达式匹配文件内容：匹配字母a-u的内容打印出来
```bash
lemu-devops@lemudevops:~/Gitlab-Runner/company-tech-doc/DevOps_Doc/Shell/Shell_Example$ grep -e "[a-u]" *csv*
lmadmin.csv:created_date,filesname,size,used,avail,file_number
lmadmin.csv:2021-08-10,Lemu_Admin,2000,100,1900,50
lmadmin.csv:2021-08-11,Lemu_Admin,2000,140,1860,100
lmadmin.csv:2021-08-12,Lemu_Admin,2000,200,1800,200
lmadmin.csv:2021-08-13,Lemu_Admin,2000,300,1700,300 
lmdevops.csv:created_date,filesname,size,used,avail,file_number
lmdevops.csv:2021-08-10,Lemu_DevOps,1700,100,1600,20
lmdevops.csv:2021-08-11,Lemu_DevOps,1700,130,1570,50
lmdevops.csv:2021-08-12,Lemu_DevOps,1700,250,1450,100 
lmdevops.csv:2021-08-13,Lemu_DevOps,1700,500,1200,300
lmtechtest.csv:created_date,filesname,size,used,avail,file_number
lmtechtest.csv:2021-08-10,Lemu_Tech,5000,100,4900,600
lmtechtest.csv:2021-08-11,Lemu_Tech,5000,150,4850,700 
lmtechtest.csv:2021-08-12,Lemu_Tech,5000,300,4700,1000
lmtechtest.csv:2021-08-13,Lemu_Tech,5000,500,4500,1500

```
### 其他示例：

| 正则表达式                                | 描述                                                                                             |
| :---------------------------------------- | :----------------------------------------------------------------------------------------------- |
| /\b([a-z]+) \1\b/gi                       | 一个单词连续出现的位置。                                                                         |
| /(\w+):\/\/([^/:]+)(:\d*)?([^# ]*)/       | 将一个URL解析为协议、域、端口及相对路径。                                                        |
| /^(?:Chapter|Section) [1-9][0-9]{0,1}$/   | 定位章节的位置。                                                                                 |
| /[-a-z]/                                  | a至z共26个字母再加一个-号。                                                                      |
| /ter\b/                                   | 可匹配chapter，而不能匹配terminal。                                                              |
| /\Bapt/                                   | 可匹配chapter，而不能匹配aptitude。                                                              |
| /Windows(?=95 |98 |NT )/                  | 可匹配Windows95或Windows98或WindowsNT，当找到一个匹配后，从Windows后面开始进行下一次的检索匹配。 |
| /^\s*$/                                   | 匹配空行。                                                                                       |
| /\d{2}-\d{5}/                             | 验证由两位数字、一个连字符再加 5 位数字组成的 ID 号。                                            |
| /<\s*(\S+)(\s[^>]*)?>[\s\S]*<\s*\/\1\s*>/ | 匹配 HTML 标记。                                                                                 |

### shell中各种括号的作用()、(())、[]、[[]]、{}
#### 单小括号()
- ①命令组。括号中的命令将会新开一个子shell顺序执行，所以括号中的变量不能够被脚本余下的部分使用。括号中多个命令之间用分号隔开，最后一个命令可以没有分号，各命令和括号之间不必有空格。
- ②命令替换。等同于`cmd`，shell扫描一遍命令行，发现了$(cmd)结构，便将$(cmd)中的cmd执行一次，得到其标准输出，再将此输出放到原来命令。有些shell不支持，如tcsh。
- ③用于初始化数组。如：array=(a b c d)

#### 双小括号(())
- ①整数扩展。这种扩展计算是整数型的计算，不支持浮点型。((exp))结构扩展并计算一个算术表达式的值，如果表达式的结果为0，那么返回的退出状态码为1，或者 是"假"，而一个非零值的表达式所返回的退出状态码将为0，或者是"true"。若是逻辑判断，表达式exp为真则为1,假则为0。
- ②只要括号中的运算符、表达式符合C语言运算规则，都可用在$((exp))中，甚至是三目运算符。作不同进位(如二进制、八进制、十六进制)运算时，输出结果全都自动转化成了十进制。如：echo $((16#5f)) 结果为95 (16进位转十进制)
- ③单纯用 (( )) 也可重定义变量值，比如 a=5; ((a++)) 可将 $a 重定义为6
- ④常用于算术运算比较，双括号中的变量可以不使用$符号前缀。括号内支持多个表达式用逗号分开。 只要括号中的表达式符合C语言运算规则,比如可以直接使用for((i=0;i<5;i++)), 如果不使用双括号, 则为for i in `seq 0 4`或者for i in {0..4}。再如可以直接使用if (($i<5)), 如果不使用双括号, 则为if [ $i -lt 5 ]。

#### 单中括号[]
- ①bash 的内部命令，[和test是等同的。如果我们不用绝对路径指明，通常我们用的都是bash自带的命令。if/test结构中的左中括号是调用test的命令标识，右中括号是关闭条件判断的。这个命令把它的参数作为比较表达式或者作为文件测试，并且根据比较的结果来返回一个退出状态码。if/test结构中并不是必须右中括号，但是新版的Bash中要求必须这样。
- ②Test和[]中可用的比较运算符只有==和!=，两者都是用于字符串比较的，不可用于整数比较，整数比较只能使用-eq，-gt这种形式。无论是字符串比较还是整数比较都不支持大于号小于号。如果实在想用，对于字符串比较可以使用转义形式，如果比较"ab"和"bc"：[ ab \< bc ]，结果为真，也就是返回状态为0。[ ]中的逻辑与和逻辑或使用-a 和-o 表示。
- ③字符范围。用作正则表达式的一部分，描述一个匹配的字符范围。作为test用途的中括号内不能使用正则。
- ④在一个array 结构的上下文中，中括号用来引用数组中每个元素的编号。

#### 双中括号[[]]
- ①[[是 bash 程序语言的关键字。并不是一个命令，[[ ]] 结构比[ ]结构更加通用。在[[和]]之间所有的字符都不会发生文件名扩展或者单词分割，但是会发生参数扩展和命令替换。
- ②支持字符串的模式匹配，使用=~操作符时甚至支持shell的正则表达式。字符串比较时可以把右边的作为一个模式，而不仅仅是一个字符串，比如[[ hello == hell? ]]，结果为真。[[ ]] 中匹配字符串或通配符，不需要引号。
- ③使用[[ ... ]]条件判断结构，而不是[ ... ]，能够防止脚本中的许多逻辑错误。比如，&&、||、<和> 操作符能够正常存在于[[ ]]条件判断结构中，但是如果出现在[ ]结构中的话，会报错。比如可以直接使用if [[ $a != 1 && $a != 2 ]], 如果不适用双括号, 则为if [ $a -ne 1] && [ $a != 2 ]或者if [ $a -ne 1 -a $a != 2 ]。
- ④bash把双中括号中的表达式看作一个单独的元素，并返回一个退出状态码。

#### 大括号{}
- ①大括号拓展。(通配(globbing))将对大括号中的文件名做扩展。在大括号中，不允许有空白，除非这个空白被引用或转义。第一种：对大括号中的以逗号分割的文件列表进行拓展。如 touch {a,b}.txt 结果为a.txt b.txt。第二种：对大括号中以点点（..）分割的顺序文件列表起拓展作用，如：touch {a..d}.sh 结果为a.txt b.txt c.txt d.txt
- ②代码块，又被称为内部组，这个结构事实上创建了一个匿名函数 。与小括号中的命令不同，大括号内的命令不会新开一个子shell运行，即脚本余下部分仍可使用括号内变量。括号内的命令间用分号隔开，最后一个也必须有分号。{}的第一个命令和左括号之间必须要有一个空格。

#### 
- ①${a} 变量a的值, 在不引起歧义的情况下可以省略大括号。
- ②$(cmd) 命令替换，和`cmd`效果相同，结果为shell命令cmd的输，过某些Shell版本不支持$()形式的命令替换, 如tcsh。
- ③$((expression)) 和`exprexpression`效果相同, 计算数学表达式exp的数值, 其中exp只要符合C语言的运算规则即可, 甚至三目运算符和逻辑表达式都可以计算。
