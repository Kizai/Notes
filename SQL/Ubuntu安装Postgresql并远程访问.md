# Ubuntu 安装Postgresql 15并远程访问

### 1.安装Postgresql
[官方安装教程](https://www.postgresql.org/download/linux/ubuntu/)
```powershell
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql
```
### 2.本地登录数据库
Postgresql安装完后会在本地创建一个postgres的用户，该用户可以无需密码登陆数据库
#### 2.1. 切换到postgres用户
```powershell
sudo -i -u postgres
```
输入`psql`进入数据库，进入成功显示如下：
```powershell
$ psql
psql (15.2 (Ubuntu 15.2-1.pgdg20.04+1))
Type "help" for help.
```
退出数据库输入`\q`

### 3. 修改配置以支持远程访问数据库
3.1. 修改postgresql的postgres用户密码
```powershell
\password postgres
```
3.2. 修改`postgresql.conf`
```powershell
sudo vim /etc/postgresql/15/main/postgresql.conf

# 第60行的 listen_addresses 注释去掉，并改成
#------------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#------------------------------------------------------------------------------

# - Connection Settings -
# 监听所有的地址
listen_addresses = '*'          # what IP address(es) to listen on;
```
3.3. 修改postgresql的配置文件,允许所有ip地址访问
```powershell
sudo vim /etc/postgresql/15/main/pg_hba.conf

# 在文末加一行
# 允许所有ip地址访问
host    all             all             0.0.0.0/0               md5
```
3.4. 退出保存，执行
```powershell
sudo  service postgresql restart
```
重启服务后即可根据ip+用户账号密码远程访问了