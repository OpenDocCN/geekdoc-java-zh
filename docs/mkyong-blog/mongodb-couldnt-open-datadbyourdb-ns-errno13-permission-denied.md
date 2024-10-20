# MongoDB:无法打开/data/db/yourdb.ns 错误号:13 权限被拒绝

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/mongodb/mongodb-couldnt-open-datadbyourdb-ns-errno13-permission-denied/>

启动 MongoDB 服务器，它在一个数据库上显示错误“权限被拒绝”,并自动关闭服务器。

```java
 $ mongod
Fri Mar  8 22:54:46 [initandlisten] MongoDB starting : pid=13492 
	port=27017 dbpath=/data/db/ 64-bit host=Yongs-MacBook-Air.local
//...
Fri Mar  8 22:54:46 [initandlisten] journal dir=/data/db/journal
Fri Mar  8 22:54:46 [initandlisten] recover : no journal files present, no recovery needed
Fri Mar  8 22:54:46 [initandlisten] couldn't open /data/db/yourdb.ns errno:13 Permission denied
Fri Mar  8 22:54:46 [initandlisten] error couldn't open file /data/db/yourdb.ns terminating
Fri Mar  8 22:54:46 dbexit: 
Fri Mar  8 22:54:46 [initandlisten] shutdown: going to close listening sockets...
Fri Mar  8 22:54:46 [initandlisten] shutdown: going to flush diaglog...
Fri Mar  8 22:54:46 [initandlisten] shutdown: going to close sockets...
Fri Mar  8 22:54:46 [initandlisten] shutdown: waiting for fs preallocator...
Fri Mar  8 22:54:46 [initandlisten] shutdown: lock for final commit...
Fri Mar  8 22:54:46 [initandlisten] shutdown: final commit...
Fri Mar  8 22:54:46 [initandlisten] shutdown: closing all files...
Fri Mar  8 22:54:46 [initandlisten] closeAllFiles() finished
Fri Mar  8 22:54:46 [initandlisten] journalCleanup...
Fri Mar  8 22:54:46 [initandlisten] removeJournalFiles
Fri Mar  8 22:54:46 [initandlisten] shutdown: removing fs lock...
Fri Mar  8 22:54:46 dbexit: really exiting now 
```

## 解决办法

错误消息显示您无权访问`yourdb.ns`数据库。查看 MongoDB 数据目录`/data/db/`，数据库`yourdb.ns`属于 root 用户。

```java
 $ ls -ls /data/db

      0 drwxr-xr-x  2 mkyong  wheel          68 Mar  8 22:54 journal
 131072 -rw-------  1 root    wheel    67108864 Mar  7 17:01 yourdb.0
 262144 -rw-------  1 root    wheel   134217728 Mar  7 16:15 yourdb.1
  32768 -rw-------  1 root    wheel    16777216 Mar  7 17:01 yourdb.ns

$whoami
mkyong 
```

若要解决此问题，请为数据库分配权限。

```java
 $ sudo chown -R mkyong /data/db

$ ls -ls /data/db

      0 drwxr-xr-x  2 mkyong  wheel          68 Mar  8 22:54 journal
 131072 -rw-------  1 mkyong  wheel    67108864 Mar  7 17:01 yourdb.0
 262144 -rw-------  1 mkyong  wheel   134217728 Mar  7 16:15 yourdb.1
  32768 -rw-------  1 mkyong  wheel    16777216 Mar  7 17:01 yourdb.ns 
```

[mongodb](http://web.archive.org/web/20190304031529/http://www.mkyong.com/tag/mongodb/)![](img/6e76e8790453332e70b8af5420a3c989.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304031529/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="12908">







