### 报错信息：
* `Package libgconf-2-4 is not installed`
* `E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)`
* `E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?`
### 解决方法：
1. 重装`libgconf-2-4 `
```shell
# 卸载libgconf-2-4 
$ sudo apt remove libgconf-2-4
$ sudo apt autoclean && sudo apt autoremove
# 安装libgconf-2-4 
sudo apt install libgconf-2-4
```
2. 删除`/var/lib/dpkg/lock-frontend `
```shell
$ sudo rm  -rf /var/lib/dpkg/lock-frontend 
```	