# MongoDB 身份验证示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/mongodb-authentication-example/>

本指南向您展示了如何在 MongoDB 中启用身份验证。默认情况下，身份验证是禁用的。要配置它，您必须首先向“admin”数据库添加一个用户。

```java
 > show dbs
admin  #add single user to this database
testdb 
```

**Note**
Users with normal access in “admin” database, HAVE read and write access to all the other databases. Users with read only access to the “admin” database HAVE only read to all databases.

这个例子使用的是 MongoDB 版本 2.2.3

## 认证示例

查看完整的示例，将“admin”用户添加到 admin 数据库，将普通用户添加到“testdb”数据库，以及如何执行身份验证。

Terminal 1 – Start MongoDB in secure mode, authentication is required.

```java
 $mongo --auth 
```

Terminal 2 – MongoDB client, see comment “#” for self-explanatory

```java
 $ mongo
MongoDB shell version: 2.2.3
connecting to: test
> use admin             		#1\. connect to the "admin" database.
switched to db admin			
> db.addUser("admin","password")	#2\. add a user "admin" to the admin database. 
{
	"user" : "admin",
	"readOnly" : false,
	"pwd" : "90f500568434c37b61c8c1ce05fdf3ae",
	"_id" : ObjectId("513af8cac115e7a6b4bcceb9")
}
addUser succeeded, but cannot wait for replication since we no longer have auth

> use testdb				#3\. connect to the "testdb" database.
switched to db testdb
> show collections			#4\. now, read and write need authentication
Sat Mar  9 16:54:57 uncaught exception: error: {
	"$err" : "unauthorized db:testdb ns:testdb.system.namespaces lock type:0 client:127.0.0.1",
	"code" : 10057
}
> use admin				#5\. connect back to the "admin" database.
switched to db admin
> db.auth("admin","password")		#6\. performs authentication, 1 means succeed, 0 means failed
1
> use testdb				#7\. connect to the "testdb" database.
switched to db testdb
> show collections			#8\. no problem, it shows all collections
system.indexes
user
> db.addUser("testdb","password")       #9\. add another user "testdb" to the "testdb" database.
{
	"user" : "testdb",
	"readOnly" : false,
	"pwd" : "b9ff75cbf18bd98d8554efec12c72090",
	"_id" : ObjectId("513af934c115e7a6b4bcceba")
}
> show collections
system.indexes
system.users				#10\. All users' data are stored in this system.users collection.
user
> db.system.users.find()
{ "_id" : ObjectId("513af934c115e7a6b4bcceba"), "user" : "testdb", "readOnly" : false, "pwd" : "b9ff75cbf18bd98d8554efec12c72090" }
> 
```

完成了。

 ## 参考

1.  [MongoDB:安全实践和管理](http://web.archive.org/web/20190308183427/http://docs.mongodb.org/manual/administration/security/)
2.  [Java MongoDB 认证示例](http://web.archive.org/web/20190308183427/http://www.mkyong.com/mongodb/java-authentication-access-to-mongodb/)

[authentication](http://web.archive.org/web/20190308183427/http://www.mkyong.com/tag/authentication/) [mongodb](http://web.archive.org/web/20190308183427/http://www.mkyong.com/tag/mongodb/)![](img/3b5f2c09681d48e0e2627a3077bcae0e.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190308183427/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="12909">







