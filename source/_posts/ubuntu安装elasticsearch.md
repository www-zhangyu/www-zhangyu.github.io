---
title: ubuntu安装elasticsearch
abbrlink: 21813
date: 2018-12-05 21:16:47
tags: ubuntu
---
## 一、Java环境
Elasticsearch是用Java语言编写的，所以首先确保机器上已经安装了Java环境。官方文档指出，至少需要Java 7，本文中使用java8。
## 二、下载Elasticsearch
在官网[https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)下载安装包，本文下载的是`elasticsearch-6.4.3.tar.gz`,将下载的安装包上传至服务器上。
## 三、解压
```
tar zxvf elasticsearch-6.4.3.tar.gz -C /usr/local
```
## 四、运行
(1) 定位至安装目录：
```
cd /usr/local/elasticsearch-6.4.3/
```
(2) 运行
```
./bin/elasticsearch
```
## 五、错误解决
### 1. java内存空间不足或文件权限不足：

```
Exception in thread "main" java.nio.file.AccessDeniedException: /usr/local/elasticsearch-6.4.3/config/jvm.options
        at sun.nio.fs.UnixException.translateToIOException(UnixException.java:84)
        at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:102)
        at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:107)
        at sun.nio.fs.UnixFileSystemProvider.newByteChannel(UnixFileSystemProvider.java:214)
        at java.nio.file.Files.newByteChannel(Files.java:361)
        at java.nio.file.Files.newByteChannel(Files.java:407)
        at java.nio.file.spi.FileSystemProvider.newInputStream(FileSystemProvider.java:384)
        at java.nio.file.Files.newInputStream(Files.java:152)
        at org.elasticsearch.tools.launchers.JvmOptionsParser.main(JvmOptionsParser.java:60)
```
解决：
(1) 切换到root账户，修改` /usr/local/elasticsearch-6.4.3/config/jvm.options`文件的Xms和Xmx的配置：
```
-Xms512m
-Xmx512m
```
(2) 修改文件权限
```
sudo chmod 644 jvm.options
```
### 2. root账号启动报错

```
org.elasticsearch.bootstrap.StartupException: java.lang.RuntimeException: can not run elasticsearch as root
        at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:140) ~[elasticsearch-6.4.3.jar:6.4.3]
        at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:127) ~[elasticsearch-6.4.3.jar:6.4.3]
        at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86) ~[elasticsearch-6.4.3.jar:6.4.3]
        at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:124) ~[elasticsearch-cli-6.4.3.jar:6.4.3]
        at org.elasticsearch.cli.Command.main(Command.java:90) ~[elasticsearch-cli-6.4.3.jar:6.4.3]
        at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:93) ~[elasticsearch-6.4.3.jar:6.4.3]
        at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:86) ~[elasticsearch-6.4.3.jar:6.4.3]
Caused by: java.lang.RuntimeException: can not run elasticsearch as root
        at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:104) ~[elasticsearch-6.4.3.jar:6.4.3]
        at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:171) ~[elasticsearch-6.4.3.jar:6.4.3]
        at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:326) ~[elasticsearch-6.4.3.jar:6.4.3]
        at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:136) ~[elasticsearch-6.4.3.jar:6.4.3]
        ... 6 more
```
解决：切换到普通用户重启启动。

### 3. 启动过程中提示

```
ERROR: [1] bootstrap checks failed
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[2018-11-23T10:00:28,057][INFO ][o.e.n.Node               ] [mQQdX2p] stopping ...
[2018-11-23T10:00:28,100][INFO ][o.e.n.Node               ] [mQQdX2p] stopped
[2018-11-23T10:00:28,101][INFO ][o.e.n.Node               ] [mQQdX2p] closing ...
[2018-11-23T10:00:28,111][INFO ][o.e.n.Node               ] [mQQdX2p] closed
```
解决：
(1) 修改配置sysctl.conf
```
vim /etc/sysctl.conf
```
(2) 添加下面配置：
```
vm.max_map_count=655360
```
(3) 配置生效：
```
sysctl -p
```
### 4. 启动过程中提示

```java
max file descriptors [65535] for elasticsearchprocess is too low, increase to at least [65536]
```
解决：
(1) 修改/etc/security/limits.conf
```
vim /etc/security/limits.conf
```
(2) 增加配置
```java
* - nofile 65536
* - memlock unlimited
```
“*”表示给所有用户起作用
## 六、设置外网访问
(1) 进入`/usr/local/elasticsearch-6.4.3/config`目录，编辑文件`elasticsearch.yml`，增加配置`network.host: 0.0.0.0`
命令如下：
```
vim  elasticsearch.yml
```
Insert进入编辑模式，增加`network.host: 0.0.0.0`，修改完成之后Esc退出编辑模式，执行`:wq`保存并退出，重启启动即可。
启动成功后，在浏览器访问：`http://IP地址:9200/`，出现以下配置表示安装成功。
```
name	"mQQdX2p"
cluster_name	"elasticsearch"
cluster_uuid	"em-fVPdIRLeG4BZAkdRarA"
version	
number	"6.4.3"
build_flavor	"default"
build_type	"tar"
build_hash	"fe40335"
build_date	"2018-10-30T23:17:19.084789Z"
build_snapshot	false
lucene_version	"7.4.0"
minimum_wire_compatibility_version	"5.6.0"
minimum_index_compatibility_version	"5.0.0"
tagline	"You Know, for Search"
```
## 七、后台运行
为了在关闭终端的时候，让Elasticsearch继续保持运行，需要设置使es在后台运行。
切换到安装目录下，执行。
```
bin/elasticsearch -d
```
