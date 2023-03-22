# Ubuntu 完全卸载 PostgresQL(适用各个版本)
### 删除相关的安装
```shell
sudo apt-get --purge remove postgresql\*
```
### 删除配置及文相关件
```powershell
sudo rm -r /etc/postgresql/
sudo rm -r /etc/postgresql-common/
sudo rm -r /var/lib/postgresql/
```
### 删除用户和所在组
```powershell
sudo userdel -r postgres
sudo groupdel postgres
```

### 重新安装
```powershell
sudo apt-get install postgresql
```