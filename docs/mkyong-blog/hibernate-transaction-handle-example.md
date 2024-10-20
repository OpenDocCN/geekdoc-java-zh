# Hibernate 事务处理示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/hibernate/hibernate-transaction-handle-example/>

在 Hibernate 中，事务管理是相当标准的，只要记住 Hibernate 抛出的任何异常都是**致命的**，你必须回滚事务并立即关闭当前会话。

下面是一个 Hibernate 事务模板:

```java
 Session session = null;
    	Transaction tx = null;

    	try{
    		session = HibernateUtil.getSessionFactory().openSession();
    		tx = session.beginTransaction();
    		tx.setTimeout(5);

    		//doSomething(session);

    		tx.commit();

    	}catch(RuntimeException e){
    		try{
    			tx.rollback();
    		}catch(RuntimeException rbe){
    			log.error("Couldn’t roll back transaction", rbe);
    		}
    		throw e;
    	}finally{
    		if(session!=null){
    			session.close();
    		}
    	} 
```

[hibernate](http://web.archive.org/web/20190309052716/http://www.mkyong.com/tag/hibernate/) [transaction](http://web.archive.org/web/20190309052716/http://www.mkyong.com/tag/transaction/)![](img/72ddc82cf8e5820fced360204b92e7c0.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190309052716/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3313">







