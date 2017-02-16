---
title: SSH的安全设置   
date: 2017-02-16   
categories: 安全   
tags: ssh   
comments: true  

---

### 前言   
在阿里云部署了一台虚拟主机，安装了zabbix，主要用于监控用户WIFI系统，平常很少登录；今天登录查看某些配置时，系统提示如下：  

![非法登录](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-001.png)
   
竟然有840次试图登录系统，真是有些担忧，需要马上加强安全。

<!-- more -->   

### 安全加固 
Linux中sshd服务的验证方式一般有2种：密码和密钥认证，平时用的最多的就是密码认证，这台zabbix的云主机就是使用密码认证，并且允许root直接远程登录；这台云机只有本人在访问，可以随意设置。

此次加固的方法：
禁用root用户远程登录，使用普通用户登录
修改为密钥认证

既然要修改为密钥登录，肯定需要一对密钥了，先介绍一下使用x-shell如何生成密钥对 
  
#### 生成密钥对   
启动x-shell程序，tools->User Key Manager

![key1](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-009.png)

![key3](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-003.png)

![key4](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-004.png)

![key6](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-005.png)

![key6](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-006.png)

![key6](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-007.png)

![key6](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-008.png)

这样密钥对生成完毕，等会需要将生成的公钥信息复制粘贴到用户的家目录内.ssh/authorized_keys文件内。      

#### 创建普通用户及授权文件
```   
# 创建用户   
useradd yxq

# 设置账号密码 
passwd yxq

# su到普通用户yxq
su - yxq

# 用户家目录下创建.ssh目录
mkdir .ssh

# 创建授权文件并将生成的公钥信息复制到文件内
vim .ssh/authorized_keys 

sh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAuTNmA/jofPrDhbChITDHpaWlt6UeD/rA/xzIckwOXdBCFDow87XsTnRtUeFZkQyH4PXgWfkSbEA7OKDvIRi0OfIUgGDf1ST1hUQRaEKnN1peKCCOZN0F6p3l6dyQHZ8BMtofmnWNRznOm/VO4I/h85LX6PPJJNxLXphyQKZIGaF4kuV6etgAMS5sqLfxfythJ6DFR4NQoxWbzYXgH+GKljvJ+8b/+SUNx07j5Lmbn0Q6CPX7Mrm7w2yKU26KfpR5yb476VZRMEpm6+UdBQ4G6pBk5rXhJ+iZ4oKPDxseydkIZZvtKO4z2uMbUcnfnd+NfSMPg1dZkwIpoI9Gy3mU/Q==
```  
 
#### 修改SSH服务的配置   
  
##### 修改配置文件/etc/ssh/sshd_config   
```   
vim /etc/ssh/sshd_config      
# 禁用root用户远程登录         
PermitRootLogin no   
   
# 禁用密码认证      
PasswordAuthentication no   

# 启用密钥认证   
PubkeyAuthentication yes   

# 密钥文件及文件路径      
AuthorizedKeysFile      .ssh/authorized_keys   
```   

##### 重启sshd服务   
```   
systemctl restart sshd.service   
```   

### 测试   
使用root登录测试      
![test1](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-010.png)      
可以看到password位置是灰色的，即时将公钥也复制到root家目录下的授权文件内也是无法登录的，因为设置了root不允许远程ssh连接登录   

使用yxq用户测试   
![test1](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-011.png)   
在User Key的下拉菜单选择ali-yxq，点ok   

![test1](http://ivixq.oss-cn-shenzhen.aliyuncs.com/blog/sshd-012.png)      
提示登录成功。






