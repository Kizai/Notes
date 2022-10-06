# CCNA学习笔记-第四节课

* 2022-08-01

### 课程的PPT

<div align=center><img src=https://s2.loli.net/2022/08/01/j9dRbnyBZs3pxSr.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/3ST7IkAoniEBbFm.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/fPSMr6nJBCWgtQu.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/QmNvfY8SZg5z1Xu.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/quHfioNUIcsRPeO.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/meDkhWsGTRrvAog.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/lqSu1fJNoin6bjr.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/q8Tlkw2KbAOxjdP.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/yHIws9QabRtlPXG.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/CyRsAJztIxnNUYZ.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/D1LFcgbqvW6UBCh.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/FL5ewz3ljEJm4NZ.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/WG1Af7UYP2JNbpS.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/WyvdoV2Ap1Zls43.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/hUDXBipaL3IYMS8.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/apkgTibEm6yBZPC.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/StZTGsV1k4uJoYE.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/g9cHFE1C2zJxl7a.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/6orKyf4YlhAJvgD.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/Ukh9fZmczWJTli4.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/wHVaJ7F4kfUMcer.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/KuoO83UbLDYeBQv.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/fMQoiNY1uRSHCzv.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/d529UwB14SVJiIX.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/TdANls2vEGM3kwz.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/UkGRs8wDbI9CJrB.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/KFx5LVbEv7HBwI6.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/nVrRBx1SmgfwOdj.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/APBwmbK9qoE6ys7.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/KkvR8Dd6Jhm7unX.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/16gMwcdDAUszjGO.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/01/2kKUD4r5vNiMPos.png width="100%" height="auto"></div>


## 实验题
### 01-Manage_IOS
* 实验目的
  * 掌握Cisco IOS命令行的基本操作
  * 掌握设备的常用基本命令
  * 设备Console信息抓取和识别

### 拓扑与需求

<div align=center><img src=https://s2.loli.net/2022/08/01/9ukTNswqCHG7Bbm.png width="100%" height="auto"></div>

### 需求：

1. 配置设备主机名，设置时区及系统时间
2. 配置“新手三条”：关闭域名解析、Console永不超时、Console权限
3. 创建MOTD
4. 设置和取消密码：Console密码、enable密码、VTY线路密码
5. 配置接口IP地址，查看接口状态
6. 直连连通性测试
7. 配置Telnet实现远程管理设备
8. 查看配置并保存
9. 删除配置至初始化状态

## 答案：
配置与实现

<div align=center><img src=https://s2.loli.net/2022/08/02/A7NcCJIWdnwYEyO.png width="100%" height="auto"></div>

设备在初次启动时，默认会进入交互式配置模式，输入‘no’ 后回车，进入正常Console 状态

### 1. 配置设备主机名，设置时区及系统时间
**R1**
```shell
Router>enable 
Router#configure terminal 
Router(config)#hostname R1
R1(config)#clock timezone GMT +8
!!修改当前系统的时区为: GMT +8 (北京)
R1(config)#exit
R1#clock set hh:mm:ss day mouth year
!!修改当前设置的时间，如：clock set 22:15:30 1 Aug 2022
```
**R2**
```shell
Router>enable 
Router#configure terminal 
Router(config)#hostname R2
R2(config)#clock timezone GMT +8
R2(config)#exit
R2#clock set 22:16:31 1 Aug 2022
```
### 2.	配置“新手三条”：关闭域名解析、Console永不超时、Console权限
**R1和R2**
```shell
Router(config)#no ip domain lookup
!!关闭域名解析
Router(config)#line console 0
Router(config-line)#no exec-time
Router(config-line)#exec-time 0 0
!!配置 console 永不超时（常用于实验环境）
Router(config-line)#privilege level 15
!!配置当console 时直接进入特权模式（常用于实验环境）
```

### 3.	创建MOTD
**R1和R2**
```shell
Router(config)#banner motd # This is My First Lab! #
!!配置提示横幅，即为登录路由器时显示的信息
```

### 4.	设置和取消密码：Console密码、enable密码、VTY线路密码
**R1和R2**

**Console密码**
```shell
Router(config)#line console 0
Router(config-line)#password word
!!配置 console 口密码，’word’ 为自己指定的密码，例如：password spoto
Router(config-line)#login
!! 使密码在 console 线路生效
Router(config-line)#no password word
!! 删除密码
```
**enable密码**
```shell
Router(config)#enable password word
!! 设置 enable 明文密码,可通过 show run 查看密码
Router(config)#no enable password
!! 取消 enable 明文密码
Router(config)#enable secret word
!! 设置 enable 密文密码,show run 只能查看到字母、数字等的组合，安全级别高于明文密码，与明文同时设置时，密文密码优先生效
Router(config)#no enable secret
!! 取消 enable 密文密码
```
**VTY线路密码**(ssh\telnet连接密码)
```shell
Router(config)#line vty 0 4
!! 进入 VTY 0-4 线路
Router(config-line)#password word
!! 设置 VTY 线路密码，即用户通过 telnet 登入路由器时输入的密码
Router(config-line)#login
!! 使密码在线路上生效
Router(config-line)#no password
Router(config-line)#no login
!! 取消 VTY 线路密码
Router(config-line)#exit
```
### 5.	配置接口IP地址，查看接口状态
**R1**
```shell
R1(config)#interface Ethernet 0/0
R1(config-if)#ip address 12.1.1.1 255.255.255.252
R1(config-if)#no shutdown
```
**R2**
```shell
R2(config)#interface Ethernet 0/0
R2(config-if)#ip address 12.1.1.2 255.255.255.252
R2(config-if)#no shutdown
```
**查看接口状态**

**使用命令 `show ip interface brief` 查看**

<div align=center><img src=https://s2.loli.net/2022/08/02/7fGJxQlWtcA6Ehk.png width="100%" height="auto"></div>

### 6.	直连连通性测试

<div align=center><img src=https://s2.loli.net/2022/08/02/rgkuEniU4yq2FV5.png width="100%" height="auto"></div>

### 7.	配置Telnet实现远程管理设备
**R1**
```shell
R1(config)#line vty 0 4
R1(config-line)#password spoto
R1(config-line)#login
R1(config-line)#transport input telnet
!!允许使用 telnet 管理
```
**R2**
```shell
R2(config)#line vty 0 4
R2(config-line)#password spoto
R2(config-line)#login
R2(config-line)#transport input telnet
```

<div align=center><img src=https://s2.loli.net/2022/08/02/UeHqjk3YnzhAatR.png width="100%" height="auto"></div>

Tips:
>> 在输入密码时不会有任何字符提示，正常输入后回车即可，注意大小写

### 8.	查看配置并保存
**查看设备配置**
```shell
Router#show running-config
!! 显示设备当前配置（配置信息保存于 RAM 中）
Router#show startup-config
!! 显示设备开机配置（配置信息保存于 NVRAM 中）
```

<div align=center><img src=https://s2.loli.net/2022/08/02/pCk7KWFQHsehg5R.png width="100%" height="auto"></div>

**保存当前配置到开机配置文件中**
```shell
Router#write
或
Router#copy running-config start-config
```

### 9.	删除配置至初始化状态
```shell
Router#erase start-config
或
Router#write erase
 
# 重启
Router#reload
```
<div align=center><img src=https://s2.loli.net/2022/08/02/WK67e3NZaOshIvL.png width="100%" height="auto"></div>

## 课后习题

1.【单选题】以下关于设备描述正确的是（ A ）
* A. 在配置模式下对设备配置进行修改以后，实际修改的是running-config文件。当设备重启以后running-config中配置将丢失
* B. 修改配置文件以后，配置将保存在startup-config中。 <font color=red>配置将保存在running-config中</font>
* C. 如果想保存修改后的配置文件，可输入命令：copy startup-config running-config <font color=red>copy running-config startup-config</font>
* D. 清除配置文件可输入命令：erase running-config <font color=red>write erase or erase startup-config</font>

2.【单选题】当命令输入错误时，想删除此命令。用以下哪个命令删除（ C ）
* A. undo
* B. shutdown
* C. no 
* D. no shutdown

3.【单选题】以下在哪个模式下键入哪条命令可查看设备版本号（ B ）
* A. Router(config)# show version
* B. Router# show version   <font color=red>特权模式 用户模式下也可以</font>
* C. Router(config-router)# show version
* D. Router(config-if)# show version

4.【单选题】以下哪个模式下可以修改网络设备配置文件（ A ）
* A. Router(config)# <font color=red>全局模式</font>
* B. Router#
* C. Router>
* D. Router：

5.【单选题】以下关于网络设备管理说法错误的是（ D ）
* A. 通过控制台可对设备进行管理，但是要求在设备现场
* B. 可通过Telnet协议对设备进行远程登录管理
* C. 可通过SSH协议对设备进行远程登录管理
* D. 可通过SSH协议远程登录设备console口对设备进行管理 <font color=red>需要连接console线缆</font>