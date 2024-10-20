# 如何从不同的目录加载 hibernate.cfg.xml

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/how-to-load-hibernate-cfg-xml-from-different-directory/>

Hibernate XML 配置文件"`hibernate.cfg.xml`"总是放在项目类路径的根目录下，在任何包之外。如果将此配置文件放在不同的目录中，可能会遇到以下错误:

```java
 Initial SessionFactory creation failed.org.hibernate.HibernateException: 
/hibernate.cfg.xml not found

Exception in thread "main" java.lang.ExceptionInInitializerError
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:25)
	at com.mkyong.persistence.HibernateUtil.<clinit>(HibernateUtil.java:8)
	at com.mkyong.common.App.main(App.java:11)
Caused by: org.hibernate.HibernateException: /hibernate.cfg.xml not found
	at org.hibernate.util.ConfigHelper.getResourceAsStream(ConfigHelper.java:147)
	at org.hibernate.cfg.Configuration.getConfigurationInputStream(Configuration.java:1405)
	at org.hibernate.cfg.Configuration.configure(Configuration.java:1427)
	at org.hibernate.cfg.Configuration.configure(Configuration.java:1414)
	at com.mkyong.persistence.HibernateUtil.buildSessionFactory(HibernateUtil.java:13)
	... 2 more 
```

要让 Hibernate 在其他目录中查找您的"`hibernate.cfg.xml`"文件，您可以通过将您的"`hibernate.cfg.xml`"文件路径作为参数传递给 **configure()** 方法来修改默认 Hibernate 的`SessionFactory`类:

```java
 SessionFactory sessionFactory = new Configuration()
            .configure("/com/mkyong/persistence/hibernate.cfg.xml")
            .buildSessionFactory();

            return sessionFactory; 
```

## HibernateUtil.java

`HibernateUtil.java`中的完整示例，从目录“ **/com/mkyong/persistence/** ”中加载“`hibernate.cfg.xml`”。

```java
 import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateUtil {

	private static final SessionFactory sessionFactory = buildSessionFactory();

	private static SessionFactory buildSessionFactory() {
		try {
			// load from different directory
			SessionFactory sessionFactory = new Configuration().configure(
					"/com/mkyong/persistence/hibernate.cfg.xml")
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

[hibernate](http://web.archive.org/web/20190306165539/http://www.mkyong.com/tag/hibernate/)![](img/9cb8ec25a5cd9418dab2687fdf62e6d6.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190306165539/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="2547">







