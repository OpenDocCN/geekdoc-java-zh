# hibernate–如果列名为关键字，如 DESC，则无法插入

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-unable-to-insert-if-column-named-is-keyword-such-as-desc/>

## 问题

MySQL 数据库中一个名为“category”的表，包含一个“ **DESC** 关键字作为列名。

```java
 CREATE TABLE `category` (
  `CATEGORY_ID` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `NAME` varchar(10) NOT NULL,
  `DESC` varchar(255) NOT NULL,
  PRIMARY KEY (`CATEGORY_ID`) USING BTREE
); 
```

*Hibernate XML 映射文件*

```java
 <hibernate-mapping>
    <class name="com.mkyong.stock.Category" table="category" catalog="mkyongdb">
        ...
        <property name="desc" type="string">
            <column name="DESC" not-null="true" />
        </property>
       ...
    </class>
</hibernate-mapping> 
```

*或休眠注释*

```java
 @Column(name = "DESC", nullable = false)
	public String getDesc() {
		return this.desc;
	} 
```

当插入到类别表中时，会出现以下错误消息:

```java
 Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: 
   You have an error in your SQL syntax; check the manual that corresponds to your 
   MySQL server version for the right syntax to use near 'DESC) 
   values ('CONSUMER', 'CONSUMER COMPANY')' at line 1
   ... 35 more 
```

 ## 解决办法

在 Hibernate 中，要插入" keyword "列名，应该像这样用'**【列名】**'括起来。

*Hibernate XML 映射文件*

```java
 <hibernate-mapping>
    <class name="com.mkyong.stock.Category" table="category" catalog="mkyongdb">
        ...
        <property name="desc" type="string">
            <column name="[DESC]" not-null="true" />
        </property>
       ...
    </class>
</hibernate-mapping> 
```

*或休眠注释*

```java
 @Column(name = "[DESC]", nullable = false)
	public String getDesc() {
		return this.desc;
	} 
```

[hibernate](http://web.archive.org/web/20190305140545/http://www.mkyong.com/tag/hibernate/) [insert](http://web.archive.org/web/20190305140545/http://www.mkyong.com/tag/insert/)![](img/29fa0f09f3adae2699e06bba1571eb95.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190305140545/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="8706">







