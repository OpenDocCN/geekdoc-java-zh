# Java MongoDB:从数据库获取集合

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/java-mongodb-get-collection-from-database/>

在 Java 中，可以使用**db . get collection(" your collection name ")**来获得一个要使用的集合。

```java
 DBCollection collection = db.getCollection("yourCollection"); 
```

如果您不知道集合名称，请使用 **db.getCollectionNames()** 从选定的数据库中获取集合名称的完整列表。

```java
 DB db = mongo.getDB("yourdb");
Set<String> collections = db.getCollectionNames();

for (String collectionName : collections) {
	System.out.println(collectionName);
} 
```

如果“yourdb”包含集合名称“yourCollection ”,那么您将看到以下结果:

```java
 system.indexes  //system collection
system.users     //system colection
yourCollection 
```

通过 Java 驱动程序从 MongoDB 获取集合的完整示例。

```java
 package com.mkyong.core;

import java.net.UnknownHostException;
import java.util.Set;

import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.Mongo;
import com.mongodb.MongoException;

/**
 * Java : Get collection from MongoDB
 * 
 */
public class GetCollectionApp {
  public static void main(String[] args) {

    try {

	Mongo mongo = new Mongo("localhost", 27017);
	DB db = mongo.getDB("yourdb");

	// get list of collections
	Set<String> collections = db.getCollectionNames();

	for (String collectionName : collections) {
		System.out.println(collectionName);
	}

	// get a single collection
	DBCollection collection = db.getCollection("yourCollection");
	System.out.println(collection.toString());

	System.out.println("Done");

    } catch (UnknownHostException e) {
	e.printStackTrace();
    } catch (MongoException e) {
	e.printStackTrace();
    }

  }
} 
```

[mongodb](http://web.archive.org/web/20190205025832/http://www.mkyong.com/tag/mongodb/) [query](http://web.archive.org/web/20190205025832/http://www.mkyong.com/tag/query/) [select](http://web.archive.org/web/20190205025832/http://www.mkyong.com/tag/select/)![](img/b12a1cf0cab7457ef015502b63193b9f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190205025832/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8797">







