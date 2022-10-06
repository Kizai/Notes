# wget命令
### 参考链接
[wget](https://www.gnu.org/software/wget/manual/wget.pdf)
### 名字    
    Wget - The non-interactive network downloader. 非交互式网络下载程序。 

### wget命令描述
- GNU Wget是一个免费实用程序，用于从Web非交互式下载文件。它支持HTTP、HTTPS和FTP协议，以及通过HTTP代理进行检索。
- Wget是非交互式的，这意味着它可以在后台工作，而用户是没有登录。这允许您启动检索和断开与系统的连接，让我们完成这项工作。相比之下，大多数Web浏览器都需要常量用户的存在，这可能是一个很大的障碍时，传输大量的数据。
- Wget可以遵循HTML、XHTML和CSS页面中的链接，以创建本地版本的远程网站，完全重新创建原始网站的目录结构。这是有时也称为“递归下载”。在这样做的同时，Wget尊重机器人排除标准(/robots.txt)。可以指示Wget将链接转换为下载的文件要指向本地文件，以便脱机查看。
- Wget的设计是为了在缓慢或不稳定的网络连接上保持健壮;如果一个由于网络问题导致下载失败，它将继续重试，直到整个文件被检索。如果服务器支持重获取，它将指示服务器从它停止的地方继续下载。 
- 1）支持断点下传功能（2）同时支持FTP和HTTP下载方式（3）支持代理服务器（4）设置方便简单；5）程序小，完全免费；
### wget命令格式
```wget [参数列表] [目标软件、网页的网址]```

#### 1、启动类参数
这一类参数主要提供软件的一些基本信息；　　
* -V,--version 显示软件版本号然后退出；
* -h,--help显示软件帮助信息；
* -e,--execute=COMMAND 执行一个 “.wgetrc”命令
以上每一个功能有长短两个参数，长短功能一样，都可以使用。需要注意的是，这里的-e参数是执行一个.wgettrc的命令，.wgettrc命令其实是一个参数列表，直接将软件需要的参数写在一起就可以了。

#### 2、文件处理参数
这类参数定义软件log文件的输出方式等；
* -o,--output-file=FILE 将软件输出信息保存到文件；
* -a,--append-output=FILE将软件输出信息追加到文件；
* -d,--debug显示输出信息；
* -q,--quiet 不显示输出信息；
* -i,--input-file=FILE 从文件中取得URL；
* 以上参数对于攻击者比较有用，我们来看看具体使用；
    * 例1：下载192.168.1.168首页并且显示下载信息
        ```wget -d http://192.168.1.168```
    * 例2：下载192.168.1.168首页并且不显示任何信息
        ```wget -q http://192.168.1.168```
    * 例3：下载filelist.txt中所包含的链接的所有文件
        ```
        wget -i filelist.txt
        wget -np -m -l5  http://lemutechnology.com //不下载本站所链接的其它站点内容，5级目录结构
        ```
        
#### 3、下载参数
下载参数定义下载重复次数、保存文件名等；
* -t,--tries=NUMBER 是否下载次数（0表示无穷次）
* -O --output-document=FILE下载文件保存为别的文件名
* -nc, --no-clobber 不要覆盖已经存在的文件
* -N,--timestamping只下载比本地新的文件
* -T,--timeout=SECONDS 设置超时时间
* -Y,--proxy=on/off 关闭代理
* 例：下载192.168.1.168的首页并将下载过程中的的输入信息保存到test.htm文件中
    ```wget -o test.htm http://192.168.1.168```

#### 4、目录参数
目录参数主要设置下载文件保存目录与原来文件（服务器文件）的目录对应关系；
* -nd  --no-directories 不建立目录
* -x,--force-directories 强制建立目录
* 可能现在我们对这里的目录还不是很了解，我们来看一个举例
* 例：下载192.168.1.168的首页，并且保持网站结构
    ```wget -x http://192.168.1.168``` 

#### 5、HTTP参数
HTTP参数设置一些与HTTP下载有关的属性；
* --http-user=USER设置HTTP用户
* --http-passwd=PASS设置HTTP密码
* --proxy-user=USER设置代理用户
* --proxy-passwd=PASS设置代理密码
* 以上参数主要设置HTTP和代理的用户、密码；

6、递归参数设置
在下载一个网站或者网站的一个目录的时候，我们需要知道的下载的层次，这些参数就可以设置；
* -r,--recursive 下载整个网站、目录（小心使用）
* -l,--level=NUMBER 下载层次
* 例：下载整个网站
   ```wget -r http://192.168.1.168```

#### 7、递归允许与拒绝选项参数
下载一个网站的时候，为了尽量快，有些文件可以选择下载，比如图片和声音，在这里可以设置；
* -A,--accept=LIST 可以接受的文件类型
* -R,--reject=LIST拒绝接受的文件类型
* -D,--domains=LIST可以接受的域名
* --exclude-domains=LIST拒绝的域名
* -L,--relative 下载关联链接
* --follow-ftp 只下载FTP链接
* -H,--span-hosts 可以下载外面的主机
* -I,--include-directories=LIST允许的目录
* -X,--exclude-directories=LIST 拒绝的目录

### wget各项参数表

| 参数                            | 含义                                                                        |
|---------------------------------|-----------------------------------------------------------------------------|
| -V,  --version                  | 显示wget的版本后退出                                                        |
| -h,  --help                     | 打印语法帮助                                                                |
| -b,  --background               | 启动后转入后台执行                                                          |
| -e,  --execute=COMMAND          | 执行`.wgetrc'格式的命令，wgetrc格式参见/etc/wgetrc或~/.wgetrc记录和输入文件 |
| -o,  --output-file=FILE         | 把记录写到FILE文件中                                                        |
| -a,  --append-output=FILE       | 把记录追加到FILE文件中                                                      |
| -d,  --debug                    | 打印调试输出                                                                |
| -q,  --quiet                    | 安静模式(没有输出)                                                          |
| -v,  --verbose                  | 冗长模式(这是缺省设置)                                                      |
| -nv, --non-verbose              | 关掉冗长模式，但不是安静模式                                                |
| -i,  --input-file=FILE          | 下载在FILE文件中出现的URLs                                                  |
| -F,  --force-html               | 把输入文件当作HTML格式文件对待                                              |
| -B,  --base=URL                 | 将URL作为在-F -i参数指定的文件中出现的相对链接的前缀                        |
| --sslcertfile=FILE              | 可选客户端证书                                                              |
| --sslcertkey=KEYFILE            | 可选客户端证书的KEYFILE                                                     |
| --egd-file=FILE                 | 指定EGD socket的文件名下载                                                  |
| --bind-address=ADDRESS          | 指定本地使用地址(主机名或IP，当本地有多个IP或名字时使用)                    |
| -t,  --tries=NUMBER             | 设定最大尝试链接次数(0 表示无限制).                                         |
| -O   --output-document=FILE     | 把文档写到FILE文件中                                                        |
| -nc, --no-clobber               | 不要覆盖存在的文件或使用.#前缀                                              |
| -c,  --continue                 | 接着下载没下载完的文件                                                      |
| --progress=TYPE                 | 设定进程条标记                                                              |
| -N,  --timestamping             | 不要重新下载文件除非比本地文件新                                            |
| -S,  --server-response          | 打印服务器的回应                                                            |
| --spider                        | 不下载任何东西                                                              |
| -T,  --timeout=SECONDS          | 设定响应超时的秒数                                                          |
| -w,  --wait=SECONDS             | 两次尝试之间间隔SECONDS秒                                                   |
| --waitretry=SECONDS             | 在重新链接之间等待1...SECONDS秒                                             |
| --random-wait                   | 在下载之间等待0...2*WAIT秒                                                  |
| -Y,  --proxy=on/off             | 打开或关闭代理                                                              |
| -Q,  --quota=NUMBER             | 设置下载的容量限制                                                          |
| --limit-rate=RATE               | 限定下载输率目录                                                            |
| -nd  --no-directories           | 不创建目录                                                                  |
| -x,  --force-directories        | 强制创建目录                                                                |
| -nH, --no-host-directories      | 不创建主机目录                                                              |
| -P,  --directory-prefix=PREFIX  | 将文件保存到目录 PREFIX/...                                                 |
| --cut-dirs=NUMBER               | 忽略 NUMBER层远程目录HTTP 选项                                              |
| --http-user=USER                | 设定HTTP用户名为 USER.                                                      |
| --http-passwd=PASS              | 设定http密码为 PASS.                                                        |
| -C,  --cache=on/off             | 允许/不允许服务器端的数据缓存 (一般情况下允许).                             |
| -E,  --html-extension           | 将所有text/html文档以.html扩展名保存                                        |
| --ignore-length                 | 忽略 `Content-Length'头域                                                   |
| --header=STRING                 | 在headers中插入字符串 STRING                                                |
| --proxy-user=USER               | 设定代理的用户名为 USER                                                     |
| --proxy-passwd=PASS             | 设定代理的密码为 PASS                                                       |
| --referer=URL                   | 在HTTP请求中包含 `Referer: URL'头                                           |
| -s,  --save-headers             | 保存HTTP头到文件                                                            |
| -U,  --user-agent=AGENT         | 设定代理的名称为 AGENT而不是 Wget/VERSION.                                  |
| --no-http-keep-alive            | 关闭 HTTP活动链接 (永远链接).                                               |
| --cookies=off                   | 不使用 cookies.                                                             |
| --load-cookies=FILE             | 在开始会话前从文件 FILE中加载cookie                                         |
| --save-cookies=FILE             | 在会话结束后将 cookies保存到 FILE文件中FTP 选项                             |
| -nr, --dont-remove-listing      | 不移走 `.listing'文件                                                       |
| -g,  --glob=on/off              | 打开或关闭文件名的 globbing机制                                             |
| --passive-ftp                   | 使用被动传输模式 (缺省值).                                                  |
| --active-ftp                    | 使用主动传输模式                                                            |
| --retr-symlinks                 | 在递归的时候，将链接指向文件(而不是目录)递归下载                            |
| -r,  --recursive                | 递归下载－－慎用!                                                           |
| -l,  --level=NUMBER             | 最大递归深度 (inf 或 0 代表无穷).                                           |
| --delete-after                  | 在现在完毕后局部删除文件                                                    |
| -k,  --convert-links            | 转换非相对链接为相对链接                                                    |
| -K,  --backup-converted         | 在转换文件X之前，将之备份为 X.orig                                          |
| -m,  --mirror                   | 等价于 -r -N -l inf -nr.                                                    |
| -p,  --page-requisites          | 下载显示HTML文件的所有图片递归下载中的包含和不包含(accept/reject)           |
| -A,  --accept=LIST              | 分号分隔的被接受扩展名的列表                                                |
| -R,  --reject=LIST              | 分号分隔的不被接受的扩展名的列表                                            |
| -D,  --domains=LIST             | 分号分隔的被接受域的列表                                                    |
| --exclude-domains=LIST          | 分号分隔的不被接受的域的列表                                                |
| --follow-ftp                    | 跟踪HTML文档中的FTP链接                                                     |
| --follow-tags=LIST              | 分号分隔的被跟踪的HTML标签的列表                                            |
| -G,  --ignore-tags=LIST         | 分号分隔的被忽略的HTML标签的列表                                            |
| -H,  --span-hosts               | 当递归时转到外部主机                                                        |
| -L,  --relative                 | 仅仅跟踪相对链接                                                            |
| -I,  --include-directories=LIST | 允许目录的列表                                                              |
| -X,  --exclude-directories=LIST | 不被包含目录的列表                                                          |
| -np, --no-parent                | 不要追溯到父目录                                                            |

### 例子
使用```wget```命令检查网页的状态码：
```bash
wget -spider -nv google.com
```


# curl命令
### 名字
curl -transfer a url :传输一个url 或叫客户端(client)的url工具。

### 参考链接
[curl](https://man7.org/linux/man-pages/man1/curl.1.html)

### 描述
curl是一种使用支持的协议（DICT、FILE、FTP、FTPS、GOPHER、HTTP、HTTPS、IMAP、IMAPS、LDAP、LDAPS、MQTT、POP3、POP3S、RTMP、 RTMPS、RTSP、SCP、SFTP、SMB、SMBS、SMTP、SMTPS、TELNET 或 TFTP）。该命令旨在无需用户交互即可工作。 

### curl各项参数表

| 参数                                    | 含义                                                      |
|-----------------------------------------|-----------------------------------------------------------|
| -a/--append                             | 上传文件时，附加到目标文件                                |
| -A/--user-agent <string>                | 设置用户代理发送给服务器                                  |
| - anyauth                               | 可以使用“任何”身份验证方法                                |
| -b/--cookie <name=string/file>          | cookie字符串或文件读取位置                                |
| - basic                                 | 使用HTTP基本验证                                          |
| -B/--use-ascii                          | 使用ASCII /文本传输                                       |
| -c/--cookie-jar                         | <file> 操作结束后把cookie写入到这个文件中                 |
| -C/--continue-at                        | <offset>  断点续转                                        |
| -d/--data <data>                        | HTTP POST方式传送数据                                     |
| --data-ascii <data>                     | 以ascii的方式post数据                                     |
| --data-binary <data>                    | 以二进制的方式post数据                                    |
| --negotiate                             | 使用HTTP身份验证                                          |
| --digest                                | 使用数字身份验证                                          |
| --disable-eprt                          | 禁止使用EPRT或LPRT                                        |
| --disable-epsv                          | 禁止使用EPSV                                              |
| -D/--dump-header <file>                 | 把header信息写入到该文件中                                |
| --egd-file <file>                       | 为随机数据(SSL)设置EGD socket路径                         |
| --tcp-nodelay                           | 使用TCP_NODELAY选项                                       |
| -e/--referer                            | 来源网址                                                  |
| -E/--cert <cert[:passwd]>               | 客户端证书文件和密码 (SSL)                                |
| --cert-type <type>                      | 证书文件类型 (DER/PEM/ENG) (SSL)                          |
| --key <key>                             | 私钥文件名 (SSL)                                          |
| --key-type <type>                       | 私钥文件类型 (DER/PEM/ENG) (SSL)                          |
| --pass  <pass>                          | 私钥密码 (SSL)                                            |
| --engine <eng>                          | 加密引擎使用 (SSL). "--engine list" for list              |
| --cacert <file>                         | CA证书 (SSL)                                              |
| --capath <directory>                    | CA目录 (made using c_rehash) to verify peer against (SSL) |
| --ciphers <list>                        | SSL密码                                                   |
| --compressed                            | 要求返回是压缩的形势 (using deflate or gzip)              |
| --connect-timeout <seconds>             | 设置最大请求时间                                          |
| --create-dirs                           | 建立本地目录的目录层次结构                                |
| --crlf                                  | 上传是把LF转变成CRLF                                      |
| -f/--fail                               | 连接失败时不显示http错误                                  |
| --ftp-create-dirs                       | 如果远程目录不存在，创建远程目录                          |
| --ftp-method [multicwd/nocwd/singlecwd] | 控制CWD的使用                                             |
| --ftp-pasv                              | 使用 PASV/EPSV 代替端口                                   |
| --ftp-skip-pasv-ip                      | 使用PASV的时候,忽略该IP地址                               |
| --ftp-ssl                               | 尝试用 SSL/TLS 来进行ftp数据传输                          |
| --ftp-ssl-reqd                          | 要求用 SSL/TLS 来进行ftp数据传输                          |
| -F/--form <name=content>                | 模拟http表单提交数据                                      |
| -form-string <name=string>              | 模拟http表单提交数据                                      |
| -g/--globoff                            | 禁用网址序列和范围使用{}和[]                              |
| -G/--get                                | 以get的方式来发送数据                                     |
| -h/--help                               | 帮助                                                      |
| -H/--header                             | <line>自定义头信息传递给服务器                            |
| --ignore-content-length                 | 忽略的HTTP头信息的长度                                    |
| -i/--include                            | 输出时包括protocol头信息                                  |
| -I/--head                               | 只显示文档信息                                            |
| - 界面<interface>                       | 指定网络接口/地址使用                                     |
| - krb4                                  | <级别>启用与指定的安全级别krb4                            |
| -j/--junk-session-cookies               | 读取文件进忽略session cookie                              |
| --interface <interface>                 | 使用指定网络接口/地址                                     |
| --krb4 <level>                          | 使用指定安全级别的krb4                                    |
| -k/--insecure                           | 允许不使用证书到SSL站点                                   |
| -K/--config                             | 指定的配置文件读取                                        |
| -l/--list-only                          | 列出ftp目录下的文件名称                                   |
| --limit-rate <rate>                     | 设置传输速度                                              |
| --local-port<NUM>                       | 强制使用本地端口号                                        |
| -m/--max-time <seconds>                 | 设置最大传输时间                                          |
| --max-redirs <num>                      | 设置最大读取的目录数                                      |
| --max-filesize <bytes>                  | 设置最大下载的文件总量                                    |
| -M/--manual                             | 显示全手动                                                |
| -n/--netrc                              | 从netrc文件中读取用户名和密码                             |
| --netrc-optional                        | 使用 .netrc 或者 URL来覆盖-n                              |
| --ntlm                                  | 使用 HTTP NTLM 身份验证                                   |
| -N/--no-buffer                          | 禁用缓冲输出                                              |
| -o/--output                             | 把输出写到该文件中                                        |
| -O/--remote-name                        | 把输出写到该文件中，保留远程文件的文件名                  |
| -p/--proxytunnel                        | 使用HTTP代理                                              |
| --proxy-anyauth                         | 选择任一代理身份验证方法                                  |
| --proxy-basic                           | 在代理上使用基本身份验证                                  |
| --proxy-digest                          | 在代理上使用数字身份验证                                  |
| --proxy-ntlm                            | 在代理上使用ntlm身份验证                                  |
| -P/--ftp-port <address>                 | 使用端口地址，而不是使用PASV                              |
| -Q/--quote <cmd>                        | 文件传输前，发送命令到服务器                              |
| -r/--range <range>                      | 检索来自HTTP/1.1或FTP服务器字节范围                       |
| --range-file                            | 读取（SSL）的随机文件                                     |
| -R/--remote-time                        | 在本地生成文件时，保留远程文件时间                        |
| --retry <num>                           | 传输出现问题时，重试的次数                                |
| --retry-delay <seconds>                 | 传输出现问题时，设置重试间隔时间                          |
| --retry-max-time <seconds>              | 传输出现问题时，设置最大重试时间                          |
| -s/--silent                             | 静音模式。不输出任何东西                                  |
| -S/--show-error                         | 显示错误                                                  |
| --socks4 <host[:port]>                  | 用socks4代理给定主机和端口                                |
| --socks5 <host[:port]>                  | 用socks5代理给定主机和端口                                |
| --stderr <file>                         |                                                           |
| -t/--telnet-option <OPT=val>            | Telnet选项设置                                            |
| --trace <file>                          | 对指定文件进行debug                                       |
| --trace-ascii <file>                    | Like --跟踪但没有hex输出                                  |
| --trace-time                            | 跟踪/详细输出时，添加时间戳                               |
| -T/--upload-file <file>                 | 上传文件                                                  |
| --url <URL>                             | Spet URL to work with                                     |
| -u/--user <user[:password]>             | 设置服务器的用户和密码                                    |
| -U/--proxy-user <user[:password]>       | 设置代理用户名和密码                                      |
| -v/--verbose                            |                                                           |
| -V/--version                            | 显示版本信息                                              |
| -w/--write-out [format]                 | 什么输出完成后                                            |
| -x/--proxy <host[:port]>                | 在给定的端口上使用HTTP代理                                |
| -X/--request <command>                  | 指定什么命令                                              |
| -y/--speed-time                         | 放弃限速所要的时间。默认为30                              |
| -Y/--speed-limit                        | 停止传输速度的限制，速度时间'秒                           |
| -z/--time-cond                          | 传送时间设置                                              |
| -0/--http1.0                            | 使用HTTP 1.0                                              |
| -1/--tlsv1                              | 使用TLSv1（SSL）                                          |
| -2/--sslv2                              | 使用SSLv2的（SSL）                                        |
| -3/--sslv3                              | 使用的SSLv3（SSL）                                        |
| --3p-quote                              | like -Q for the source URL for 3rd party transfer         |
| --3p-url                                | 使用url，进行第三方传送                                   |
| --3p-user                               | 使用用户名和密码，进行第三方传送                          |
| -4/--ipv4                               | 使用IP4                                                   |
| -6/--ipv6                               | 使用IP6                                                   |
| -#/--progress-bar                       | 用进度条显示当前的传送状态                                |
### 例子
```bash
#使用curl命令检查http服务器的状态
#-m设置curl不管访问成功或失败，最大消耗的时间为5秒，5秒连接服务为相应则视为无法连接
#-s设置静默连接，不显示连接时的连接速度、时间消耗等信息
#-o将curl下载的页面内容导出到/dev/null(默认会在屏幕显示页面内容)
#-w设置curl命令需要显示的内容%{http_code}，指定curl返回服务器的状态码
curl -m 5 -s-o /dev/null -w %{http_code} https://www.google.com
```

# lwp-request命令
[lwp-request](https://linux.die.net/man/1/lwp-request)
### 名字
lwp-request, GET, POST, HEAD - 简单的命令行用户代理
### 描述
lwp-request可用于向WWW服务器和您的本地文件系统发送请求。POST和 PUT方法的请求内容是从 stdin 读取的。响应的内容打印在标准输出上。错误消息打印在 stderr 上。该程序返回一个状态值，指示失败的 URL 的数量。

### HEAD 命令参数选项

| 命令                     | 含义                                                 |
|--------------------------|------------------------------------------------------|
| -m <method>              | use method for the request (default is 'HEAD')       |
| -f                       | make request even if HEAD believes method is illegal |
| -b <base>                | Use the specified URL as base                        |
| -t <timeout>             | Set timeout value                                    |
| -i <time>                | Set the If-Modified-Since header on the request      |
| -c <conttype>            | use this content-type for POST, PUT, CHECKIN         |
| -a                       | Use text mode for content I/O                        |
| -p <proxyurl>            | use this as a proxy                                  |
| -P                       | don't load proxy settings from environment           |
| -H <header>              | send this HTTP header (you can specify several)      |
| -C <username>:<password> | provide credentials for basic authentication         |
| -u                       | Display method and URL before any response           |
| -U                       | Display request headers (implies -u)                 |
| -s                       | Display response status code                         |
| -S                       | Display response status chain (implies -u)           |
| -e                       | Display response headers (implies -s)                |
| -E                       | Display whole chain of headers (implies -S and -U)   |
| -d                       | Do not display content                               |
| -o <format>              | Process HTML content in various ways                 |
| -v                       | Show program version                                 |
| -h                       | Print this message                                   |