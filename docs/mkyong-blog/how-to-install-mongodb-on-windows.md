# 如何在 Windows 上安装 MongoDB

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/how-to-install-mongodb-on-windows/>

在本教程中，我们将向您展示如何在 Windows 上安装 **MongoDB** 。

1.  MongoDB 2.2.3
2.  Windows 7

**Note**
The MongoDB does not require installation, just download and extracts the zip file, configure the data directory and start it with command “`mongod`“.

## 1.下载 MongoDB

从官方 [MongoDB 网站](http://web.archive.org/web/20221225035510/https://www.mongodb.org/downloads)下载 MongoDB。选择 Windows 32 位或 64 位。解压，解压到你喜欢的位置，例如:`d:\mongodb\`。

## 2.查看 MongoDB 文件夹

在 MongoDB 中，它在 bin 文件夹中只包含 10+个可执行文件(exe)。这是真的，这是 MongoDB 所需的文件，对于像我这样来自关系数据库背景的开发人员来说，这真的很难相信。

*图:$MongoDB/bin 文件夹下的文件*

![mongodb-windows](img/dc4ebd1ae030bc3ff1b0690ba8deac4a.png)**Note**
It’s recommended to add `$MongoDB/bin` to Windows environment variable, so that you can access the MongoDB’s commands in command prompt easily.

## 3.配置文件

创建一个 MongoDB 配置文件，它只是一个文本文件，例如:`d:\mongodb\mongo.config`

d:\mongodb\mongo.config

```java
 ##store data here
dbpath=D:\mongodb\data

##all output go here
logpath=D:\mongodb\log\mongo.log

##log read and write operations
diaglog=3 
```

**Note**
MongoDB need a folder (data directory) to store its data. By default, it will store in “`C:\data\db`“, create this folder manually. MongoDB won’t create it for you. You can also specify an alternate data directory with `--dbpath` option.

## 4.运行 MongoDB 服务器

使用`mongod.exe --config d:\mongodb\mongo.config`启动 MongoDB 服务器。

```java
 d:\mongodb\bin>mongod --config D:\mongodb\mongo.config
all output going to: D:\mongodb\log\mongo.log 
```

## 5.连接到 MongoDB

使用`mongo.exe`连接到启动的 MongoDB 服务器。

```java
 d:\mongodb\bin>mongo
MongoDB shell version: 2.2.3
connecting to: test
> //mongodb shell 
```

## 6.MongoDB 作为 Windows 服务

将 MongoDB 添加为 Windows 服务，这样 MongoDB 将在每次系统重启后自动启动。

使用`--install`安装为 Windows 服务。

```java
 d:\mongodb\bin> mongod --config D:\mongodb\mongo.config --install 
```

一个名为“MongoDB”的 Windows 服务被创建。

![mongodb-windows-service](img/28ce1bfc9a317f37cff3781d6a6b8739.png)

要启动 MongoDB 服务

```java
 net start MongoDB 
```

停止 MongoDB 服务

```java
 net stop MongoDB 
```

删除 MongoDB 服务

```java
 d:\mongodb\bin>mongod --remove 
```

## 7.常见问题

1.在 Windows 8 上安装 MongoDB 作为 Windows 服务，但点击“访问被拒绝。”错误消息:

```java
 C:\Users\mkyong2002>mongod --config D:\mongodb\mongo.config --install
Tue Jul 16 21:05:55.154 diagLogging level=3
Tue Jul 16 21:05:55.155 diagLogging using file D:\mongodb\data/diaglog.51e54533
Tue Jul 16 21:05:55.155 Trying to install Windows service 'MongoDB'
Tue Jul 16 21:05:55.155 Error connecting to the Service Control Manager: Access
is denied. (5) 
```

要修复它，请使用管理权限运行命令提示符——右键单击命令提示符图标，选择以管理员身份运行。

## 参考

1.  [在 Windows 上安装 MongoDB](http://web.archive.org/web/20221225035510/http://docs.mongodb.org/manual/tutorial/install-mongodb-on-windows/)
2.  [MongoDB 配置选项](http://web.archive.org/web/20221225035510/http://docs.mongodb.org/manual/reference/configuration-options/)

<input type="hidden" id="mkyong-current-postId" value="8755">