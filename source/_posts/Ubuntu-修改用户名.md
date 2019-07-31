---
title: Ubuntu 修改用户名
tags: ubuntu
abbrlink: 3158
date: 2019-02-11 14:38:50
password:
top:
---
ubuntu系统中将用户名test改为test1。
** 注意：首先将当前登录用户切换为root或其他用户，否则在第二步中会出现密码不正确的提示。**
```
su - root
```
## 1. 修改 /etc/passwd用户信息文件
```
vim /etc/passwd
```
将test改为test1
## 2. 修改 /etc/shadow用户密码文件
将test改为test1
## 3. 修改 /etc/group用户组文件 
将所有test改为test1
## 4. 修改用户的家目录 
mv /home/test /home/test1
以上，就将用户名修改成功了。