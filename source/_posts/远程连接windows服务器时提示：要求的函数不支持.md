---
title: 远程连接windows服务器时提示：要求的函数不支持
abbrlink: 34587
date: 2018-12-06 08:25:53
tags:
---
win10系统远程连接windows服务器时提示下面图片中的问题：
![这里写图片描述](https://img-blog.csdn.net/20180518140655487?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM3MzkwNzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

解决办法：
打开注册表，
将\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\CredSSP\Parameters路径下的AllowEncryptionOracle的值改为2。
如果该路径下的某个项找不到，新建即可。
新建数值名称AllowEncryptionOracle时，类型应为DWORD(32位)值，如果选择64位该设置不生效。