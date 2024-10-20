# 如何以编程方式添加 Hibernate XML 映射文件(hbm.xml)

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-add-hibernate-xml-mapping-file-hbm-xml-programmatically/>

Hibernate XML 映射文件包含 Java 类和数据库表之间的映射关系。这总是被命名为“xx.hbm.xml”，并在 Hibernate 配置文件“hibernate.cfg.xml”中声明。

例如，映射文件(hbm.xml)在“**映射**标签中声明

```java
 <?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
<session-factory>
  <property name="hibernate.bytecode.use_reflection_optimizer">false</property>
  <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
  <property name="hibernate.connection.password">password</property>
  <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mkyong</property>
  <property name="hibernate.connection.username">root</property>
  <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
  <property name="show_sql">true</property>
  <mapping resource="com/mkyong/common/Stock.hbm.xml"></mapping>
</session-factory>
</hibernate-configuration> 
```

## 以编程方式添加 Hibernate 的映射文件(hbm.xml)

出于任何原因，您不想将映射文件包含在`hibernate.cfg.xml`中。Hibernate 为开发人员提供了一种以编程方式添加映射文件的方法。

只需修改默认的 Hibernate `SessionFactory`类，将您的“ **hbm.xml** ”文件路径作为参数传递给`addResource()`方法:

```java
 SessionFactory sessionFactory = new Configuration()
   .addResource("com/mkyong/common/Stock.hbm.xml")
   .buildSessionFactory(); 
```

 ## HibernateUtil.java

`HibernateUtil.java`完整示例，以编程方式加载 Hibernate XML 映射文件“xx.hbm.xml”。

```java
 import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {

	private static final SessionFactory sessionFactory = buildSessionFactory();

	private static SessionFactory buildSessionFactory() {
		try {

			SessionFactory sessionFactory = new Configuration()
					.configure("/com/mkyong/persistence/hibernate.cfg.xml")
					.addResource("com/mkyong/common/Stock.hbm.xml")
					.buildSessionFactory();

			return sessionFactory;

		} catch (Throwable ex) {
			// Make sure you log the exception, as it might be swallowed
			System.err.println("Initial SessionFactory creation failed." + ex);
			throw new ExceptionInInitializerError(ex);
		}
	}

	public static SessionFactory getSessionFactory() {
		return sessionFactory;
	}

	public static void shutdown() {
		// Close caches and connection pools
		getSessionFactory().close();
	}

} 
```

完成了。

[hibernate](http://web.archive.org/web/20190213132705/http://www.mkyong.com/tag/hibernate/)![](img/d5437aab03a6db5cf36f2c8b08309abb.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190213132705/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="2551">







