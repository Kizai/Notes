### 前言 
 之前在AWS AMI的系统去部署python服务，遇到了一个比较头疼的问题，在使用pip命令时会报ssl不可用的问题，究其原因是因为openssl的版本太老了，所以需要对openssl的版本进行更新，如果使用的是python3版本的服务，可能还需要重装python3，下面是我自己的解决方案，希望对你们有帮助。
### 解决步骤
```shell
$ wget https://www.openssl.org/source/openssl-1.1.1a.tar.gz
$ tar -zxvf openssl-1.1.1a.tar.gz
$ cd openssl-1.1.1a
# 编译安装
$ ./config --prefix=/usr/local/openssl no-zlib #不需要zlib
$ make
$ make install
# 备份原配置
$ sudo mv /usr/bin/openssl /usr/bin/openssl.bak
$ sudo mv /usr/include/openssl/ /usr/include/openssl.bak
# 新版配置
$ sudo ln -s /usr/local/openssl/include/openssl /usr/include/openssl
$ sudo ln -s /usr/local/openssl/lib/libssl.so.1.1 /usr/local/lib64/libssl.so
$ sudo ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl
# 修改系统配置
# 写入openssl库文件的搜索路径
$ sudo echo "/usr/local/openssl/lib" >> /etc/ld.so.conf
# 使修改后的/etc/ld.so.conf生效
$ sudo ldconfig -v

# 我这里安装的是python3.8.10版本，如需要修改其他版本，请自己查找对应版本包的链接进行修改。
# 安装python3.8.10
$ wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
$ tar zxvf Python-3.8.10.tgz
$ cd Python-3.8.10
$ sudo yum install gcc
$ ./configure --prefix=/opt/python3
$ ./configure --with-openssl=/usr/local/openssl
$ make
$ sudo yum install openssl-devel
$ ./configure --with-openssl=/usr/local/openssl
$ sudo make install
$ sudo ln -s /opt/python3/bin/python3 /usr/bin/python3
# 更新pip3
$ python3 -m pip install --upgrade pip
```
看不懂指令的有注释说明！觉得有用麻烦一键三连！！！谢谢！
