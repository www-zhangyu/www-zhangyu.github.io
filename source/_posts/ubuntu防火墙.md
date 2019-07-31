---
title: ubuntu防火墙
abbrlink: 26683
date: 2019-01-08 17:26:22
tags: ubuntu
password:
top:
---
## 1. 安装
```
sudo apt-get install ufw
```
## 2. 开启
```
sudo ufw enable
```
## 3. 默认关闭所有外部访问
```
sudo ufw default deny
```
## 4. 查看ufw端口规则
```
sudo ufw status 
```
## 5. 新增规则
```
sudo ufw allow 8080
```
or
```
sudo ufw deny 8080
```
## 6. 只允许指定ip地址访问指定端口
- 入站规则
```
sudo iptables -A INPUT -s 10.2.72.114 -p tcp --dport 22 -j ACCEPT
```
- 出站规则
```
sudo iptables -A OUTPUT -d xxx.xxx.xxx.xxx -p tcp --sport 22 -j ACCEPT
```
## 7. 删除上面建立的某条规则 
```
sudo ufw delete allow smtp
```
## 8. 关闭防火墙
```
sudo ufw disable 
```

qkxsb130@qkxsb-virtual-machine:~$ sudo ufw allow 9411
规则已添加
规则已添加 (v6)
qkxsb130@qkxsb-virtual-machine:~$ sudo ufw allow 5672
规则已添加
规则已添加 (v6)
qkxsb130@qkxsb-virtual-machine:~$ sudo ufw allow 15672
规则已添加
规则已添加 (v6)
qkxsb130@qkxsb-virtual-machine:~$ sudo ufw allow 9200
规则已添加
规则已添加 (v6)
qkxsb130@qkxsb-virtual-machine:~$ sudo ufw allow 9300