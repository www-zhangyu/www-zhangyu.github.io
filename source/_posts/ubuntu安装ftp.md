---
title: ubuntu安装ftp
abbrlink: 17401
date: 2018-12-06 13:50:53
tags: ubuntu
---
### 1. install
```
sudo apt-get install vsftpd
```
### 2. 配置文件详解
```
sudo vim /etc/vsftpd.conf
```
内容为：
```

listen=NO
listen_ipv6=YES
# 控制是否允许匿名用户登入,YES为允许匿名登入，NO为不允许,默认yes
anonymous_enable=NO  

#控制是否允许本地用户登入，YES为允许本地用户登入，NO为不允许。默认值为YES。 
local_enable=YES

 # 是否允许登陆用户有写权限。属于全局设置，默认值为YES。
write_enable=YES

#设置匿名登入者新增或上传档案时的umask值。默认值为077，则新建档案的对应权限为700。 
local_umask=022

#是否允许匿名登入者有上传文件（非目录）的权限，只有在write_enable=YES时，此项才有效。默认值为NO。 
anon_upload_enable=YES 

#允许匿名登入者有新增目录的权限，只有在write_enable=YES时，此项才有效。默认值为NO。 
#anon_mkdir_write_enable=YES

# 欢迎语设置 ：如果启动这个选项，那么使用者第一次进入一个目录时，会检查该目录下是否有.message这个档案，如果有，则会出现此档案的内容，通常这个档案会放置欢迎话语，或是对该目录的说明。默认值为开启。 
dirmessage_enable=YES

#如果启用，vsftpd在显示目录资源列表的时候，在显示你的本地时间。而默认的是显示GMT（格林尼治时间）。默认为NO
use_localtime=YES

#是否记录ftp传输过程  
xferlog_enable=YES

#指定FTP使用20端口进行数据传输，默认值为YES
connect_from_port_20=YES

#设置是否改变匿名用户上传文件（非目录）的属主。默认值为NO
#chown_uploads=YES
#设置匿名用户上传文件（非目录）的属主名。建议不要设置为root。 
#chown_username=whoever

#设置日志文件名和路径
#xferlog_file=/var/log/vsftpd.log

#如果启用，则日志文件将会写成xferlog的标准格式，如同wu-ftpd一般。默认值为关闭
#xferlog_std_format=YES

#设置多长时间不对FTP服务器进行任何操作，则断开该FTP连接，单位为秒。默认值为300
#idle_session_timeout=600

#设置建立FTP数据连接的超时时间，单位为秒。默认值为120
#data_connection_timeout=120

#运行vsftpd需要的非特权系统用户
#nopriv_user=ftpsecure

# 是否允许运行特殊的ftp命令
#async_abor_enable=YES

#设置是否启用ASCII模式上传数据。默认值为NO。 
#ascii_upload_enable=YES

#设置是否启用ASCII模式下载数据。默认值为NO。 
#ascii_download_enable=YES

#定制欢迎信息  
#ftpd_banner=Welcome to blah FTP service.

# 若是启动这项功能，则必须提供一个档案/etc/vsftpd/banner_emails，内容为email address。若是使用匿名登入，则会要求输入email address，若输入的email address在此档案内，则不允许进入。默认值为NO。 
#deny_email_enable=YES
 
# 此文件用来输入email address，只有在deny_email_enable=YES时，才会使用到此档案。若是使用匿名登入，则会要求输入email address，若输入的email address 在此档案内，则不允许进入。
#banned_email_file=/etc/vsftpd.banned_emails

#用于指定用户列表文件中的用户是否允许切换到上级目录。默认值为NO
chroot_local_user=YES
#只能访问自身所属目录
allow_writeable_chroot=YES

#在默认配置下，本地用户登入FTP后可以使用cd命令切换到其他目录，这样会对系统带来安全隐患。可以通过以下三条配置文件来控制用户切换目录。 
#用于指定用户列表文件，该文件用于控制哪些用户可以切换到用户家目录的上级目录。 
##chroot_local_user=YES
#控制用户是否允许切换到上级目录 
#chroot_list_enable=YES
#设置是否启用chroot_list_file配置项指定的用户列表文件。默认值为NO。 
#chroot_list_file=/etc/vsftpd.chroot_list

#若是启用此功能，则允许登入者使用ls –R（可以查看当前目录下子目录中的文件）这个指令
#ls_recurse_enable=YES

#这个设置指定了一个空目录，这个目录不允许ftp user写入。在vsftpd不希望文件系统被访问时，目录为安全的虚根所使用。
secure_chroot_dir=/var/run/vsftpd/empty

#这个设置指定了SSL加密连接需要的RSA证书的位置。 
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key

#如果启用，vsftpd将启用openSSL，通过SSL支持安全连接。这个设置用来控制连接（包括登录）和数据线路。同时，你的客户端也要支持SSL才行
ssl_enable=NO


#utf8_filesystem=YES
```
- chroot_list_enable、chroot_list_file、chroot_local_user搭配实现的效果：
①当chroot_list_enable=YES，chroot_local_user=YES时，在/etc/vsftpd.chroot_list文件中列出的用户，可以切换到其他目录；未在文件中列出的用户，不能切换到其他目录。 
②当chroot_list_enable=YES，chroot_local_user=NO时，在/etc/vsftpd.chroot_list文件中列出的用户，不能切换到其他目录；未在文件中列出的用户，可以切换到其他目录。 
③当chroot_list_enable=NO，chroot_local_user=YES时，所有的用户均不能切换到其他目录。 
④当chroot_list_enable=NO，chroot_local_user=NO时，所有的用户均可以切换到其他目录。 

### 3. 限制用户不能访问
编辑`/etc/ftpusers`文件，将不允许访问的用户名加入该文件中，重启。
```
service vsftpd restart
```