# MongoDB 导入和导出示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/mongodb-import-and-export-example/>

在本教程中，我们将向您展示如何使用命令:`mongoexport`和`mongoimport`备份和恢复 MongoDB。

## 1.使用 mongoexport 备份数据库

几个例子向你展示如何使用`mongoexport`来备份数据库。

回顾一些常用选项。

```java
 $ mongoexport
Export MongoDB data to CSV, TSV or JSON files.

options:
  -h [ --host ] arg         mongo host to connect to ( <set name>/s1,s2 for 
  -u [ --username ] arg     username
  -p [ --password ] arg     password
  -d [ --db ] arg           database to use
  -c [ --collection ] arg   collection to use (some commands)
  -q [ --query ] arg        query filter, as a JSON string
  -o [ --out ] arg          output file; if not specified, stdout is used 
```

1.1 将所有文档(所有字段)导出到文件“`domain-bk.json`”。

```java
 $ mongoexport -d webmitta -c domain -o domain-bk.json
connected to: 127.0.0.1
exported 10951 records 
```

1.2 仅导出所有带“`domain`”和“`worth`”字段的文件。

```java
 $ mongoexport -d webmitta -c domain -f "domain,worth" -o domain-bk.json
connected to: 127.0.0.1
exported 10951 records 
```

1.3 使用搜索查询导出所有文档，在这种情况下，只会导出带有“`worth > 100000`”的文档。

```java
 $mongoexport -d webmitta -c domain -f "domain,worth" -q '{worth:{$gt:100000}}' -o domain-bk.json
connected to: 127.0.0.1
exported 10903 records 
```

1.4 连接到远程服务器，如 mongolab.com，使用用户名和密码。

```java
 $ mongoexport -h id.mongolab.com:47307 -d heroku_app -c domain -u username123 -p password123 -o domain-bk.json
connected to: id.mongolab.com:47307
exported 10951 records 
```

查看导出的文件。

```java
 $ ls -lsa
total 2144
   0 drwxr-xr-x   5 mkyong  staff      170 Apr 10 12:00 .
   0 drwxr-xr-x+ 50 mkyong  staff     1700 Apr  5 10:55 ..
2128 -rw-r--r--   1 mkyong  staff  1089198 Apr 10 12:15 domain-bk.json 
```

**Note**
With `mongoexport`, all exported documents will be in json format. ## 2.使用 mongoimport 恢复数据库

几个例子向你展示如何使用`mongoimport`来恢复数据库。

回顾一些常用选项。

```java
 $ mongoimport
connected to: 127.0.0.1
no collection specified!
Import CSV, TSV or JSON data into MongoDB.

options:
  -h [ --host ] arg       mongo host to connect to ( <set name>/s1,s2 for sets)
  -u [ --username ] arg   username
  -p [ --password ] arg   password
  -d [ --db ] arg         database to use
  -c [ --collection ] arg collection to use (some commands)
  -f [ --fields ] arg     comma separated list of field names e.g. -f name,age
  --file arg              file to import from; if not specified stdin is used
  --drop                  drop collection first 
  --upsert                insert or update objects that already exist 
```

2.1 将文件“`domain-bk.json`”中的所有文档导入名为“webmitta2.domain2”的 database.collection 中。将自动创建所有不存在的数据库或集合。

```java
 $ mongoimport -d webmitta2 -c domain2 --file domain-bk.json
connected to: 127.0.0.1
Wed Apr 10 13:26:12 imported 10903 objects 
```

2.2 导入所有文档，插入或更新已经存在的对象(基于`_id`)。

```java
 $ mongoimport -d webmitta2 -c domain2 --file domain-bk.json --upsert
connected to: 127.0.0.1
Wed Apr 10 13:26:12 imported 10903 objects 
```

2.3 使用用户名和密码连接到远程服务器 mongolab.com，并将本地文件`domain-bk.json`中的文档导入远程 MongoDB 服务器。

```java
 $ mongoimport -h id.mongolab.com:47307 -d heroku_app -c domain -u username123 -p password123 --file domain-bk.json
connected to: id.mongolab.com:47307
Wed Apr 10 13:26:12 imported 10903 objects 
```

 ## 参考

1.  [MongoDB 官方文档-导入导出 MongoDB 数据](http://web.archive.org/web/20190304032805/http://docs.mongodb.org/manual/core/import-export/)

[backup](http://web.archive.org/web/20190304032805/http://www.mkyong.com/tag/backup/) [export](http://web.archive.org/web/20190304032805/http://www.mkyong.com/tag/export/) [import](http://web.archive.org/web/20190304032805/http://www.mkyong.com/tag/import/) [mongodb](http://web.archive.org/web/20190304032805/http://www.mkyong.com/tag/mongodb/)







