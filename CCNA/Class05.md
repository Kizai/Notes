# CCNA学习笔记-第五节课

* 2022-08-03

## 课程的PPT

<div align=center><img src=https://s2.loli.net/2022/08/03/plTEv4x7L8zhXeI.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/UZN5V4xdHnDkEWl.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/NfVTknZBtMboXqh.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/TSmVPHyCQ9afnzq.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/Ck1HOiTQwRMqJ6m.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/sxpyqbQNEwe1Co6.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/PhObpzUmIiGJeEH.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/Q3zw7jy1YXWfnRl.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/hUFV64KAP9tQ5Zq.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/oLCdn1r8bHWwlUx.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/ztH4ODTNhaZK16W.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/ZVKkTuqn1HDtPwE.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/aM7xbdiAPv5erEc.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/exyP2fC9RGnUqpW.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/2jL1ZYiMBUIDcKa.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/QcYSJ3DKUWHdZp9.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/95sjHP2gwvUtmQi.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/QnbeksLEfmtUuvH.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/j1mSf5RCcNKrbG9.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/Qe6XAN9Mw1VUa2n.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/8vCTxrNW7PuUbVm.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/8ybcK1odrOtRBga.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/1S4CmZ7ocfXrAdT.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/K72GC53mUoufFOL.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/8sxJQc4qytW7dDw.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/Xp9ShbBMvkILn5g.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/GXIZtEjoJ8qsh7W.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/wmcLXUtajNFT8sE.png width="100%" height="auto"></div>


## 实验题
**02-Switch_Vlan**
### 实验目的
* 掌握交换机VLAN的基本操作
* 理解广播域的作用

### 拓扑与需求
**拓扑：**

<div align=center><img src=https://s2.loli.net/2022/08/03/DVza2wI5G8nT1EW.png width="100%" height="auto"></div>

**需求：**
1. 设置SW1和SW2的设备名分别为SW1和SW2
2. 按拓扑图所示配置PC1~4的IP地址
3. 交换机按图示配置各终端所属的相应vlan，并且进行合理的配置使得同vlan间PC可以相互访问，不同vlan间PC不可以相互访问（不使用trunk）
4. 在SW2上配置使得PC1可以使用Telnet远程管理到SW2，远程管理密码为SPOTO

### 配置与实现
1. 设置SW1和SW2的设备名分别为SW1和SW2

**SW1**
```shell
Switch>enable
Switch#configure terminal
Switch(config)#hostname SW1
```
**SW2**
```shell
Switch>enable
Switch#configure terminal
Switch(config)#hostname SW2
```

2. 按拓扑图所示配置PC1~4的IP地址

**PC1**
```shell
PC1>enable
PC1#configure terminal
PC1(config)#interface Ethernet0/0
PC1(config-if)#ip address 192.168.10.1 255.255.255.0
PC1(config-if)#no shutdown
```
**PC2**
```shell
PC2>enable
PC2#configure terminal
PC2(config)#interface Ethernet0/0
PC2(config-if)#ip address 192.168.20.1 255.255.255.0
PC2(config-if)#no shutdown
```
**PC3**
```shell
PC3>enable
PC3#configure terminal
PC3(config)#interface Ethernet0/0
PC3(config-if)#ip address 192.168.10.2 255.255.255.0
PC3(config-if)#no shutdown
```
**PC4**
```shell
PC4>enable
PC4#configure terminal
PC4(config)#interface Ethernet0/0
PC4(config-if)#ip address 192.168.20.2 255.255.255.0
PC4(config-if)#no shutdown
```

3. 交换机按图示配置各终端所属的相应vlan，并且进行合理的配置使得同vlan间PC可以相互访问，不同vlan间PC不可以相互访问（不使用trunk）

**SW1**
```shell
SW1(config)#interface range ethernet 0/0-1
SW1(config-if-range)#switchport mode access 
SW1(config-if-range)#switchport access vlan 10
% Access VLAN does not exist. Creating vlan 10
SW1(config-if-range)#interface range ethernet 0/2-3
SW1(config-if-range)#switchport mode access 
SW1(config-if-range)#switchport access vlan 20
% Access VLAN does not exist. Creating vlan 20
```
>> 当接口划入的VLAN不存在时，IOS会自动创建

>> 使用 range 可以批量操作多个接口

**SW2**
```shell
SW2(config)#interface range ethernet 0/0-1
SW2(config-if-range)#switchport mode access 
SW2(config-if-range)#switchport access vlan 10
% Access VLAN does not exist. Creating vlan 10
SW2(config-if-range)#interface range ethernet 0/2-3
SW2(config-if-range)#switchport mode access 
SW2(config-if-range)#switchport access vlan 20
% Access VLAN does not exist. Creating vlan 20
```
<div align=center><img src=https://s2.loli.net/2022/08/03/QmdJY7NOt4c59Tj.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/x9jyiDfEmTw1c3v.png width="100%" height="auto"></div>

4. 在SW2上配置使得PC1可以使用Telnet远程管理到SW2，远程管理密码为`SPOTO`

**SW2**
>> 配置SVI（交换虚拟接口，可用于管理交换机）
```shell
SW2(config)#interface vlan 10
SW2(config-if)#ip address 192.168.10.3 255.255.255.0
SW2(config-if)#no shutdown
```
>> 配置Enable密码为SPOTO

>> 不设置enable密码将不允许远程管理

```shell
SW2(config)#enable password SPOTO
```

<div align=center><img src=https://s2.loli.net/2022/08/03/5txnVAfOLI4M38y.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/nqTtjhzkp2JU8KB.png width="100%" height="auto"></div>

>> 注意密码，区分大小写

---------------------------------------------------
**03-VTP**
### 实验目的
* 掌握Trunk的配置
* 掌握VTP基本操作
* 理解VTP角色之间的区别

### 拓扑与需求

<div align=center><img src=https://s2.loli.net/2022/08/03/jyRZ3hQeiPtaTBG.png width="100%" height="auto"></div>

### 需求：

1. SW1、SW2和SW3之间的线路需配置为Trunk，采用Dot1q封装协议
2. SW1为VTP Server模式，SW2为VTP Transparent模式，SW3为VTP Client模式，Domain名为 SPOTO， VTP密码为 P@s5w0rd
3. 在SW1上创建VLAN 10 名字为VTP-Server，在SW2上创建VLAN 20 名字为VTP-Transparent，观察SW1~3的VLAN数据库以及VTP状态

### 配置与实现
1. SW1、SW2和SW3之间的线路需配置为Trunk，采用Dot1q封装协议

**SW1 & SW2**
```shell
SWX(config)#interface ethernet 0/0
SWX(config-if)#switchport trunk encapsulation dot1q 
SWX(config-if)#switchport mode trunk
```
**SW2 & SW3**
```shell
SWX(config)#interface ethernet 0/1
SWX(config-if)#switchport trunk encapsulation dot1q 
SWX(config-if)#switchport mode trunk
```

2. SW1为`VTP Server`模式，SW2为`VTP Transparent`模式，SW3为`VTP Client`模式，Domain名为 `SPOTO`， VTP密码为 `P@s5w0rd` ，使用VTP 版本`2`

**SW1**
```shell
SW1(config)#vtp version 2
SW1(config)#vtp mode server
Device mode already VTP Server for VLANS.
SW1(config)#vtp domain SPOTO
Changing VTP domain name from NULL to SPOTO
SW1(config)#vtp password P@s5w0rd
Setting device VTP password to P@s5w0rd
```
>> IOS默认VTP模式为Server，域名和密码为空

**SW2**
```shell
SW2(config)#vtp version 2
SW2(config)#vtp mode transparent 
Setting device to VTP Transparent mode for VLANS.
SW2(config)#vtp domain SPOTO
Domain name already set to SPOTO.
SW2(config)#vtp password P@s5w0rd
Setting device VTP password to P@s5w0rd
```

3. 在SW1上创建`VLAN 10` 名字为`VTP-Server`，在SW2上创建`VLAN 20` 名字为`VTP-Transparent`，观察SW1~3的VLAN数据库以及VTP状态

**SW1**
```shell
SW1(config)#vlan 10
SW1(config-vlan)#name VTP-Server
```

**SW2**
```shell
SW2(config)#vlan 20
SW2(config-vlan)#name VTP-Transparent
```

<div align=center><img src=https://s2.loli.net/2022/08/03/9tgCF8ead4LiwoQ.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/6Pfmc1igYKEajDT.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/lZ2TYwGP1Sou7jR.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/cw4AqM1Fh7mKEux.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/cw4AqM1Fh7mKEux.png width="100%" height="auto"></div>

<div align=center><img src=https://s2.loli.net/2022/08/03/MpBG8dnFNlh2iTR.png width="100%" height="auto"></div>

>> 此处出现*** MD5 digest checksum mismatch on trunk: Et0/0 *** 消息，不需理会，SW2为透明模式，不会同步和被同步SW1的VLAN数据库，所以MD5校验不一致也是正常现象

<div align=center><img src=https://s2.loli.net/2022/08/03/kQySBms4tTLYewD.png width="100%" height="auto"></div>

>> 查看SW1和SW3的MD5 digest部分，如果此部分完全一致，则表示VTP已经同步

## 课后习题

1.【单选题】关于VLAN 1描述错误的是（ D ）
* A. vlan 1 是默认vlan不可删除
* B. vlan 1是默认本征vlan
* C. 交换机默认情况下所有接口均属于vlan 1
* D. MAC地址表中不会学习VLAN 1中的终端物理地址

2.【单选题】关于trunk链路描述错误的是（ D ）
* A. Native vlan两端需一致，否则可能会出现vlan跳转攻击
* B. 封装ISL协议原理是在数据帧前封装ISL头部
* C. 使用Dot1q协议封装trunk链路以后除native vlan以外，其余vlan均需要打tag
* D. 在进行trunk链路封装时，一端采用ISL一端采用dot1q是可行的

3.【单选题】可以用来承载多个vlan信息的接口模式为（ B ）
* A. access接口模式
* B. trunk接口模式
* C. 三层接口模式
* D. 以上三种均不是

4.【单选题】关于VLAN的特点描述错误的是（ C ）
* A. 一个VLAN中所有设备都在一个广播域内
* B. 广播报文不可跨VLAN传播
* C. 不同VLAN间用户可以直接进行通信，无须借助其他网络设备
* D. VLAN工作于数据链路层

5.【单选题】以下描述错误的是（ D ）
* A. 集线器所有接口均属于一个冲突域，一个广播域
* B. 交换机所有接口默认均属于一个广播域，每一个接口属于一个独立冲突域
* C. 路由器每个接口属于独立的冲突域，每个接口属于独立的广播域
* D. 网桥所有接口属于一个冲突域，所有接口属于一个广播域