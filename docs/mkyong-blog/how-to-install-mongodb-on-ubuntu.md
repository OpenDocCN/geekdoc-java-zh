# 如何在 Ubuntu 上安装 mongoDB

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/how-to-install-mongodb-on-ubuntu/>

![mongodb-ubuntu](img/3b7c2619c1a1dab116c9274e484bdbb5.png)

本指南向您展示了如何在 Ubuntu 上安装 MongoDB。

1.  Ubuntu 12.10
2.  MongoDB 2.2.3

## 1.将 10gen 包添加到 source.list.d

Ubuntu 12 自带“mongo”包，但不是最新版本。

```java
 $ sudo apt-cache search mongodb
mongodb
mongodb-clients
mongodb-dev
mongodb-server 
```

建议给`/etc/apt/sources.list.d`加 10gen 包，因为里面有最新稳定的 MongoDB。创建一个`/etc/apt/sources.list.d/mongo.list`文件，并声明 10gen 发行版。

```java
 $ sudo vim /etc/apt/sources.list.d/mongo.list 
```

/etc/apt/sources.list.d/mongo.list

```java
 ##10gen package location

deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen 
```

## 2.添加 GPG 键

第 10 代产品包需要 GPG 密钥，将其导入:

```java
 $ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10 
```

如果您没有导入 GPG 键，`apt-get update`将会显示以下错误信息:

```java
 GPG error: http://downloads-distro.mongodb.org dist Release: 
The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 9ECBEC467F0CEB10 
```

## 3.更新包

更新您的`apt-get`列表。

```java
 $ sudo apt-get update 
```

再次搜索“mongodb”，现在出现了新的 10gen 包。获取“`mongodb-10gen`”，它包含了最新稳定的 MongoDB。

```java
 $ sudo apt-cache search mongodb
mongodb
mongodb-clients
mongodb-dev
mongodb-server

mongodb-10gen
mongodb18-10gen
mongodb20-10gen 
```

## 4.安装 mongodb-10gen

一切准备就绪，现在您可以安装 MongoDB:

```java
 $ sudo apt-get install mongodb-10gen 
```

## 5.MongoDB 在哪里？

MongoDB 已安装并启动。

```java
 $ ps -ef | grep mongo
mongodb   5262     1  0 15:27 ?        00:00:14 /usr/bin/mongod --config /etc/mongodb.conf
mkyong    5578  3994  0 16:29 pts/0    00:00:00 grep --color=auto mongo

$ mongo -version
MongoDB shell version: 2.2.3 
```

所有的 MongoDB 可执行文件都存储在`/usr/bin/`

```java
 $ ls -ls /usr/bin | grep mongo
 4220 -rwxr-xr-x 1 root   root     4317928 Feb  2 08:11 mongo
10316 -rwxr-xr-x 1 root   root    10563336 Feb  2 08:11 mongod
10320 -rwxr-xr-x 1 root   root    10563664 Feb  2 08:11 mongodump
10284 -rwxr-xr-x 1 root   root    10526736 Feb  2 08:11 mongoexport
10324 -rwxr-xr-x 1 root   root    10567768 Feb  2 08:11 mongofiles
10296 -rwxr-xr-x 1 root   root    10539056 Feb  2 08:11 mongoimport
10272 -rwxr-xr-x 1 root   root    10514544 Feb  2 08:11 mongooplog
10272 -rwxr-xr-x 1 root   root    10518512 Feb  2 08:11 mongoperf
10320 -rwxr-xr-x 1 root   root    10563632 Feb  2 08:11 mongorestore
 6644 -rwxr-xr-x 1 root   root     6802848 Feb  2 08:11 mongos
10312 -rwxr-xr-x 1 root   root    10556560 Feb  2 08:11 mongostat
10272 -rwxr-xr-x 1 root   root    10515856 Feb  2 08:11 mongotop 
```

“mongodb 控制脚本”在`/etc/init.d/mongodb`生成

```java
 $ ls -ls /etc/init.d | grep mongo
 0 lrwxrwxrwx 1 root root   21 Feb  2 08:11 mongodb -> /lib/init/upstart-job 
```

MongoDB 配置文件位于`/etc/mongodb.conf`

/etc/mongodb.conf

```java
 # mongodb.conf

# Where to store the data.

# Note: if you run mongodb as a non-root user (recommended) you may
# need to create and set permissions for this directory manually,
# e.g., if the parent directory isn't mutable by the mongodb user.
dbpath=/var/lib/mongodb

#where to log
logpath=/var/log/mongodb/mongodb.log

logappend=true

#port = 27017

#...... 
```

## 6.控制 MongoDB

一些控制 MongoDB 的命令。

正在启动 MongoDB

```java
 $ sudo service mongodb start 
```

停止 mongodb

```java
 $ sudo service mongodb stop 
```

重新启动 MongoDB

```java
 $ sudo service mongodb restart 
```

## 参考

1.  [在 Ubuntu 上安装 MongoDB 的官方指南](http://web.archive.org/web/20220814143922/http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)
2.  [Debian Linux apt-get 包管理备忘单](http://web.archive.org/web/20220814143922/http://www.cyberciti.biz/tips/linux-debian-package-management-cheat-sheet.html)
3.  [在 Mac OS X 上安装 MongoDB](http://web.archive.org/web/20220814143922/http://www.mkyong.com/mongodb/how-to-install-mongodb-on-mac-os-x/)
4.  [在 Windows 上安装 MongoDB](http://web.archive.org/web/20220814143922/http://www.mkyong.com/mongodb/how-to-install-mongodb-on-windows/)

<input type="hidden" id="mkyong-current-postId" value="8909">