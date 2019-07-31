---
title: Ubuntu服务器安装gitolite
tags: ubuntu
abbrlink: 35976
date: 2018-12-06 08:25:00
---
## 一、安装git、sshopen-server、sshopen-client
```
sudo apt-get update
sudo apt-get install git openssh-server openssh-client
```
## 二、将本地公钥文件上传到服务器
windows下公钥文件地址：`C:\Users\用户\.ssh`
将上面路径下的id_rsa.pub文件上传到服务器/tmp路径下
## 三、服务器端创建git用户
```
sudo adduser --system --shell /bin/bash --gecos 'GIT SCM User' --group --disabled-password  --home /home/git git
```
## 四、安装gitolite

### 1. 切换到git用户
```
su - git
```
### 2.  创建文件夹bin
```
mkdir bin
```
### 3. 从github克隆gitolite的源码
```
 git clone https://github.com/sitaramc/gitolite.git
```
### 4. 安装gitolite
```
./gitolite/install -to /home/git/bin/
```
### 5. 为gitolite配置管理员
```
/home/git/bin/gitolite setup -pk /tmp/admin.pub
```
至此gitolite的安装配置完成，可以查看bin目录里的内容。
