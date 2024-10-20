# MongoDB hello world 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/mongodb-hello-world-example/>

![mongodb hello world](img/b6bc11eefd853229032c92a9f8304936.png "mongodb-nosql")

一个向你展示如何在 MongoDB 中创建、更新、查找、删除记录和索引等基本操作的快速指南。这个例子使用的是 MongoDB 2.0.7，运行在 Mac OS X 10.8 上，MongoDB 客户端和服务器控制台都运行在同一台机器的本地主机上。

## 1.安装 MongoDB

在 [Windows](http://web.archive.org/web/20190224144752/http://www.mkyong.com/mongodb/how-to-install-mongodb-on-windows/) 、 [Ubuntu](http://web.archive.org/web/20190224144752/http://www.mkyong.com/mongodb/how-to-install-mongodb-on-ubuntu/) 或 [Mac OS X](http://web.archive.org/web/20190224144752/http://www.mkyong.com/mongodb/how-to-install-mongodb-on-mac-os-x/) 上安装 MongoDB。安装很容易，基本上只需下载 MongoDB zip 文件，然后运行命令-`$MongoDB-folder/bin/mongod`。

使用`mongod`启动 MongoDB。

```java
 $./mongod
Tue Sep 11 21:55:36 [initandlisten] MongoDB starting : 
pid=72280 port=27017 dbpath=/data/db/ 64-bit host=Yongs-MacBook-Air.local
Tue Sep 11 21:55:36 [initandlisten] db version v2.0.7, pdfile version 4.5
Tue Sep 11 21:55:36 [initandlisten] options: {}
Tue Sep 11 21:55:36 [initandlisten] journal dir=/data/db/journal
Tue Sep 11 21:55:36 [initandlisten] recover : no journal files present, no recovery needed
Tue Sep 11 21:55:36 [websvr] admin web console waiting for connections on port 28017
Tue Sep 11 21:55:36 [initandlisten] waiting for connections on port 27017 
```

 ## 2.连接 MongoDB

为了连接 MongoDB，使用了`$MongoDB-folder/bin/mongo`

```java
 $ ./mongo
MongoDB shell version: 2.0.7
connecting to: test 
```

 ## 3.创建数据库或表(集合)

在 MongoDB 中，数据库和表都是在第一次插入数据时自动创建的。使用`use database-name`，切换到您的数据库(即使这个数据库还没有创建)。

在下面的例子中，插入一条记录后，数据库“mkyong”和表“users”被动态创建。

```java
 $ ./mongo
MongoDB shell version: 2.0.7
connecting to: test
> use mkyong
switched to db mkyong

> db.users.insert({username:"mkyong",password:"123456"})
> db.users.find()
{ "_id" : ObjectId("504f45cd17f6c778042c3c07"), "username" : "mkyong", "password" : "123456" } 
```

你应该知道的三个数据库命令。

1.  `show dbs`–列出所有数据库。
2.  `use db_name`–切换到数据库名称。
3.  `show collections`–列出当前所选数据库中的所有表格。

**Note**
In MongoDB, **collection** means **table** in SQL.

## 4.插入记录

要插入一条记录，使用`db.tablename.insert({data})`或`db.tablename.save({data})`，两者都可以，不知道为什么 MongoDB 创建了两者。

```java
 > db.users.save({username:"google",password:"google123"})
> db.users.find()
{ "_id" : ObjectId("504f45cd17f6c778042c3c07"), "username" : "mkyong", "password" : "123456" }
{ "_id" : ObjectId("504f48ea17f6c778042c3c0a"), "username" : "google", "password" : "google123" } 
```

## 5.更新记录

要更新记录，使用`db.tablename.update({criteria},{$set: {new value}})`。在下面的例子中，用户名“mkyong”的密码被更新。

```java
 > db.users.update({username:"mkyong"},{$set:{password:"hello123"}})
> db.users.find()
{ "_id" : ObjectId("504f48ea17f6c778042c3c0a"), "username" : "google", "password" : "google123" }
{ "_id" : ObjectId("504f45cd17f6c778042c3c07"), "password" : "hello123", "username" : "mkyong" } 
```

## 6.查找记录

要查找或查询记录，使用`db.tablename.find({criteria})`。

**6.1** 列出表格“用户”中的所有记录。

```java
 > db.users.find()
{ "_id" : ObjectId("504f48ea17f6c778042c3c0a"), "username" : "google", "password" : "google123" }
{ "_id" : ObjectId("504f45cd17f6c778042c3c07"), "password" : "hello123", "username" : "mkyong" } 
```

**6.2** 查找用户名为“google”的记录

```java
 > db.users.find({username:"google"})
{ "_id" : ObjectId("504f48ea17f6c778042c3c0a"), "username" : "google", "password" : "google123" } 
```

**6.3** 查找用户名长度小于或等于 2 的记录

```java
 db.users.find({$where:"this.username.length<=2"}) 
```

**6.4** 查找存在用户名字段的记录。

```java
 db.users.find({username:{$exists : true}}) 
```

## 7.删除记录

要删除记录，使用`db.tablename.remove({criteria})`。在下面的例子中，用户名“google”的记录被删除。

```java
 > db.users.remove({username:"google"})
> db.users.find()
{ "_id" : ObjectId("504f45cd17f6c778042c3c07"), "password" : "hello123", "username" : "mkyong" } 
```

**Note**
To delete all records from a table, uses `db.tablename.remove()`.
To drop the table, uses `db.tablename.drop()`.

## 8.索引

索引可以帮助你提高查询数据的速度。

**8.1** 列出“用户”表的所有索引，默认情况下“_id”列始终是主键，是自动创建的。

```java
 > db.users.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"ns" : "mkyong.users",
		"name" : "_id_"
	}
]
> 
```

**8.2** 创建一个索引，使用`db.tablename.ensureIndex(column)`。在下面的示例中，在“用户名”列上创建了一个索引。

```java
 > db.users.ensureIndex({username:1})
> db.users.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"ns" : "mkyong.users",
		"name" : "_id_"
	},
	{
		"v" : 1,
		"key" : {
			"username" : 1
		},
		"ns" : "mkyong.users",
		"name" : "username_1"
	}
] 
```

**8.3** 要降一个指标，使用`db.tablename.dropIndex(column)`。在下面的示例中，列“username”上的索引被删除或丢弃。

```java
 > db.users.dropIndex({username:1})
{ "nIndexesWas" : 2, "ok" : 1 }
> db.users.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"ns" : "mkyong.users",
		"name" : "_id_"
	}
]
> 
```

**8.4** 创建一个唯一的索引，使用`db.tablename.ensureIndex({column},{unique:true})`。在下面的示例中，在“用户名”列上创建了一个唯一索引。

```java
 > db.users.ensureIndex({username:1},{unique:true});
> db.users.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"ns" : "mkyong.users",
		"name" : "_id_"
	},
	{
		"v" : 1,
		"key" : {
			"username" : 1
		},
		"unique" : true,
		"ns" : "mkyong.users",
		"name" : "username_1"
	}
] 
```

## 10.帮助

最后，使用`help()`来指导你如何使用 MongoDB。

10.1 `help` -所有可用的命令。

```java
 > help
	db.help()                    help on db methods
	db.mycoll.help()             help on collection methods
	rs.help()                    help on replica set methods
	help admin                   administrative help
	help connect                 connecting to a db help
	help keys                    key shortcuts  
	//... 
```

10.2 `db.help()` -显示关于数据库的帮助。

```java
 > db.help()
DB methods:
	db.addUser(username, password[, readOnly=false])
	db.auth(username, password)
	db.cloneDatabase(fromhost)
	db.commandHelp(name) returns the help for the command
	db.copyDatabase(fromdb, todb, fromhost)
	//... 
```

10.3 `db.collection.help()` -显示关于收集的帮助(表格)。

```java
 > db.users.help()
DBCollection help
	db.users.find().help() - show DBCursor help
	db.users.count()
	db.users.dataSize()
	db.users.distinct( key ) - eg. db.users.distinct( 'x' )
	db.users.drop() drop the collection
	db.users.dropIndex(name)
	//... 
```

10.4 `db.collection.function.help()` -显示功能帮助。

```java
 > db.users.find().help()
find() modifiers
	.sort( {...} )
	.limit( n )
	.skip( n )
	.count() - total # of objects matching query, ignores skip,limit
	.size() - total # of objects cursor would return, honors skip,limit
	.explain([verbose])
    //... 
```

完成了。希望 MongoDB 命令的总结可以帮助其他人。

## 参考

1.  [官方 MongoDB 教程](http://web.archive.org/web/20190224144752/http://www.mongodb.org/display/DOCS/Tutorial) 
2.  [MongoDB 索引](http://web.archive.org/web/20190224144752/http://www.mongodb.org/display/DOCS/Indexes)

[hello world](http://web.archive.org/web/20190224144752/http://www.mkyong.com/tag/hello-world/) [mongodb](http://web.archive.org/web/20190224144752/http://www.mkyong.com/tag/mongodb/)







