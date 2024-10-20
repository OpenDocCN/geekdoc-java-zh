# Hibernate 拦截器示例–审计日志

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-interceptor-example-audit-log/>

Hibernate 有一个强大的特性叫做“**拦截器**”，用来拦截或挂钩不同类型的 Hibernate 事件，比如数据库 CRUD 操作。在本文中，我将演示如何使用 Hibernate 拦截器实现一个应用程序审计日志特性，它将把所有 Hibernate 的保存、更新或删除操作记录到一个名为“**审计日志**的数据库表中。

## Hibernate 拦截器示例–审计日志

## 1.创建表格

创建一个名为“审计日志”的表来存储所有应用程序审计记录。

```java
 DROP TABLE IF EXISTS `mkyong`.`auditlog`;
CREATE TABLE  `mkyong`.`auditlog` (
  `AUDIT_LOG_ID` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `ACTION` varchar(100) NOT NULL,
  `DETAIL` text NOT NULL,
  `CREATED_DATE` date NOT NULL,
  `ENTITY_ID` bigint(20) unsigned NOT NULL,
  `ENTITY_NAME` varchar(255) NOT NULL,
  PRIMARY KEY (`AUDIT_LOG_ID`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8; 
```

## 2.创建标记界面

创建一个标记接口，任何实现这个接口的类都将被审计。这个接口要求实现的类公开它的标识符-**getId()**和要记录的内容-**getLogDeatil()**。所有暴露的数据将被存储到数据库中。

```java
 package com.mkyong.interceptor;
//market interface
public interface IAuditLog {

	public Long getId();	
	public String getLogDeatil();
} 
```

## 3.映射“审计日志”表

要与表“审计日志”映射的普通注释模型文件。

```java
 @Entity
@Table(name = "auditlog", catalog = "mkyong")
public class AuditLog implements java.io.Serializable {

	private Long auditLogId;
	private String action;
	private String detail;
	private Date createdDate;
	private long entityId;
	private String entityName;
        ...
} 
```

## 4.一个类实现了 IAuditLog

一个普通的注释模型文件，用于映射表“stock ”,稍后将用于截取程序演示。它必须实现 **IAuditLog** 标记接口，并实现 **getId()** 和 **getLogDeatil()** 方法。

```java
 ...
@Entity
@Table(name = "stock", catalog = "mkyong"
public class Stock implements java.io.Serializable, IAuditLog {
...
        @Transient
	@Override
	public Long getId(){
		return this.stockId.longValue();
	}

	@Transient
	@Override
	public String getLogDeatil(){
		StringBuilder sb = new StringBuilder();
		sb.append(" Stock Id : ").append(stockId)
		.append(" Stock Code : ").append(stockCode)
		.append(" Stock Name : ").append(stockName);

		return sb.toString();
	}
... 
```

## 5.创建一个助手类

一个助手类，用于接收拦截器的数据，并将其存储到数据库中。

```java
 ...
public class AuditLogUtil{

   public static void LogIt(String action,
     IAuditLog entity, Connection conn ){

     Session tempSession = HibernateUtil.getSessionFactory().openSession(conn);

     try {

	AuditLog auditRecord = new AuditLog(action,entity.getLogDeatil()
		, new Date(),entity.getId(), entity.getClass().toString());
	tempSession.save(auditRecord);
	tempSession.flush();

     } finally {	
	tempSession.close();		
     }		
  }
} 
```

## 6.创建一个 Hibernate 拦截器类

通过扩展 Hibernate**empty interceptor**创建一个拦截器类。下面是最流行的拦截器函数。

*   on save–在保存对象时调用，对象尚未保存到数据库中。
*   onflush dirty–当您更新一个对象时调用，该对象尚未更新到数据库中。
*   on delete–当您删除一个对象时调用，该对象尚未被删除到数据库中。
*   预刷新–在保存、更新或删除的对象提交到数据库之前调用(通常在后刷新之前)。
*   post flush–在保存、更新或删除的对象提交到数据库后调用。

代码相当冗长，它应该是自我探索的。

```java
 ...
public class AuditLogInterceptor extends EmptyInterceptor{

	Session session;
	private Set inserts = new HashSet();
	private Set updates = new HashSet();
	private Set deletes = new HashSet();

	public void setSession(Session session) {
		this.session=session;
	}

	public boolean onSave(Object entity,Serializable id,
		Object[] state,String[] propertyNames,Type[] types)
		throws CallbackException {

		System.out.println("onSave");

		if (entity instanceof IAuditLog){
			inserts.add(entity);
		}
		return false;

	}

	public boolean onFlushDirty(Object entity,Serializable id,
		Object[] currentState,Object[] previousState,
		String[] propertyNames,Type[] types)
		throws CallbackException {

		System.out.println("onFlushDirty");

		if (entity instanceof IAuditLog){
			updates.add(entity);
		}
		return false;

	}

	public void onDelete(Object entity, Serializable id, 
		Object[] state, String[] propertyNames, 
		Type[] types) {

		System.out.println("onDelete");

		if (entity instanceof IAuditLog){
			deletes.add(entity);
		}
	}

	//called before commit into database
	public void preFlush(Iterator iterator) {
		System.out.println("preFlush");
	}	

	//called after committed into database
	public void postFlush(Iterator iterator) {
		System.out.println("postFlush");

	try{

		for (Iterator it = inserts.iterator(); it.hasNext();) {
		    IAuditLog entity = (IAuditLog) it.next();
		    System.out.println("postFlush - insert");		
		    AuditLogUtil.LogIt("Saved",entity, session.connection());
		}	

		for (Iterator it = updates.iterator(); it.hasNext();) {
		    IAuditLog entity = (IAuditLog) it.next();
		    System.out.println("postFlush - update");
		    AuditLogUtil.LogIt("Updated",entity, session.connection());
		}	

		for (Iterator it = deletes.iterator(); it.hasNext();) {
		    IAuditLog entity = (IAuditLog) it.next();
		    System.out.println("postFlush - delete");
		    AuditLogUtil.LogIt("Deleted",entity, session.connection());
		}	

	} finally {
		inserts.clear();
		updates.clear();
		deletes.clear();
	}
       }		
} 
```

## 7.启用拦截器

您可以通过将拦截器作为参数传递给 **openSession(拦截器)来启用拦截器；**。

```java
 ...
   Session session = null;
   Transaction tx = null;
   try {

	AuditLogInterceptor interceptor = new AuditLogInterceptor();
	session = HibernateUtil.getSessionFactory().openSession(interceptor);
	interceptor.setSession(session);

	//test insert
	tx = session.beginTransaction();
	Stock stockInsert = new Stock();
	stockInsert.setStockCode("1111");
	stockInsert.setStockName("mkyong");
	session.saveOrUpdate(stockInsert);
	tx.commit();

	//test update
	tx = session.beginTransaction();
	Query query = session.createQuery("from Stock where stockCode = '1111'");
	Stock stockUpdate = (Stock)query.list().get(0);
	stockUpdate.setStockName("mkyong-update");
	session.saveOrUpdate(stockUpdate);
	tx.commit();

	//test delete
	tx = session.beginTransaction();
	session.delete(stockUpdate);
	tx.commit();

   } catch (RuntimeException e) {
	try {
		tx.rollback();
	} catch (RuntimeException rbe) {
		// log.error("Couldn’t roll back transaction", rbe);
   }
	throw e;
   } finally {
	if (session != null) {
		session.close();
	}
   }
... 
```

在插入测试中

```java
 session.saveOrUpdate(stockInsert); //it will call onSave
tx.commit(); // it will call preFlush follow by postFlush 
```

在更新测试中

```java
 session.saveOrUpdate(stockUpdate); //it will call onFlushDirty
tx.commit(); // it will call preFlush follow by postFlush 
```

在删除测试中

```java
 session.delete(stockUpdate); //it will call onDelete
tx.commit();  // it will call preFlush follow by postFlush 
```

##### 输出

```java
 onSave
Hibernate: 
    insert into mkyong.stock
    (STOCK_CODE, STOCK_NAME) 
    values (?, ?)
preFlush
postFlush
postFlush - insert
Hibernate: 
    insert into mkyong.auditlog
    (ACTION, CREATED_DATE, DETAIL, ENTITY_ID, ENTITY_NAME) 
    values (?, ?, ?, ?, ?)
preFlush
Hibernate: 
    select ...
    from mkyong.stock stock0_ 
    where stock0_.STOCK_CODE='1111'
preFlush
onFlushDirty
Hibernate: 
    update mkyong.stock 
    set STOCK_CODE=?, STOCK_NAME=? 
    where STOCK_ID=?
postFlush
postFlush - update
Hibernate: 
    insert into mkyong.auditlog
    (ACTION, CREATED_DATE, DETAIL, ENTITY_ID, ENTITY_NAME) 
    values (?, ?, ?, ?, ?)
onDelete
preFlush
Hibernate: 
    delete from mkyong.stock where STOCK_ID=?
postFlush
postFlush - delete
Hibernate: 
    insert into mkyong.auditlog 
    (ACTION, CREATED_DATE, DETAIL, ENTITY_ID, ENTITY_NAME) 
    values (?, ?, ?, ?, ?) 
```

##### 在数据库中

```java
 SELECT * FROM auditlog a; 
```

所有审计数据都被插入数据库。

![interceptor-example](img/15209c3c95b5312739628aa8c48f79e2.png "interceptor-example")

## 结论

审计日志是一个有用的特性，通常在数据库中使用触发器来处理，但是出于可移植性的考虑，我建议使用应用程序来实现它。

Download this example – [Hibernate interceptor example.zip](http://web.archive.org/web/20210506191153/http://www.mkyong.com/wp-content/uploads/2010/02/HibernateInterceptotExample.zip)Tags : [hibernate](http://web.archive.org/web/20210506191153/https://mkyong.com/tag/hibernate/) [interceptor](http://web.archive.org/web/20210506191153/https://mkyong.com/tag/interceptor/)<input type="hidden" id="mkyong-current-postId" value="3340">