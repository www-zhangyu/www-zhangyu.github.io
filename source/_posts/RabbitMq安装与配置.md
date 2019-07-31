---
title: RabbitMq安装与配置
tags: ubuntu
abbrlink: 6666
date: 2018-12-06 08:23:25
---
### 1. 安装
#### 1.1 添加源 
```
echo 'deb http://www.rabbitmq.com/debian/ testing main' | sudo tee /etc/apt/sources.list.d/rabbitmq.list
```
#### 1.2 新增公钥（不加会有警告） 
```
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -
```
#### 1.3 更新源 
```
sudo apt-get update
```
#### 1.4 安装rabbitmq-server
```
sudo apt-get install rabbitmq-server
```
#### 1.5 查看rabbotMQ的状态
```
invoke-rc.d rabbitmq-server status 
```
#### 1.6 打开管理页面 
```
sudo rabbitmq-plugins enable rabbitmq_management
```
#### 1.7 查看安装的插件 
```
sudo rabbitmqctl list_users
```
### 2. 配置新增用户可以远程访问 
#### 2.1 新增用户
```
sudo rabbitmqctl add_user admin admin 
sudo rabbitmqctl set_user_tags admin administrator
```
#### 2.2 允许远程访问
```
sudo rabbitmqctl  set_permissions  -p  '/'  admin '.' '.' '.'
sudo service rabbitmq-server restart 
```

