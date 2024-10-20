# Struts + Hibernate 集成示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-hibernate-integration-example/>

一个教程，展示了如何在用 Apache Struts 1.x 开发的 web 应用程序中集成 Hibernate。

Download It – [Struts-Hibernate-Example.zip](http://web.archive.org/web/20190214221344/http://www.mkyong.com/wp-content/uploads/2010/04/Struts-Hibernate-Example.zip)

**整合步骤:**

1.  创建一个新的 **Hibernate Struts 插件**文件，在 servlet 上下文中设置 Hibernate 会话工厂，并将该文件包含在 **struts-config.xml** 文件中。
2.  在 Struts 中，从 servlet 上下文中获取 **Hibernate 会话工厂，并执行您想要的任何 Hibernate 任务。**

## 1.Hibernate Struts 插件

创建一个 Hibernate Struts 插件，获取 Hibernate 会话工厂，将其存储到 servlet 上下文中，供以后的用户使用—**servlet . getservletcontext()。setAttribute(KEY_NAME，factory)；**。

```java
 package com.mkyong.common.plugin;

import java.net.URL;
import javax.servlet.ServletException;

import org.apache.struts.action.ActionServlet;
import org.apache.struts.action.PlugIn;
import org.apache.struts.config.ModuleConfig;
import org.hibernate.HibernateException;
import org.hibernate.MappingException;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernatePlugin implements PlugIn {
   private Configuration config;
   private SessionFactory factory;
   private String path = "/hibernate.cfg.xml";
   private static Class clazz = HibernatePlugin.class;

   public static final String KEY_NAME = clazz.getName();

   public void setPath(String path) {
      this.path = path;
   }

   public void init(ActionServlet servlet, ModuleConfig modConfig)
      throws ServletException {

      try {

    	 //save the Hibernate session factory into serlvet context
         URL url = HibernatePlugin.class.getResource(path);
         config = new Configuration().configure(url);
         factory = config.buildSessionFactory();
         servlet.getServletContext().setAttribute(KEY_NAME, factory);

      } catch (MappingException e) {
         throw new ServletException();
      } catch (HibernateException e) {
         throw new ServletException();
      }

   }

   public void destroy() {
      try {
         factory.close();
      } catch (HibernateException e) {
         e.printStackTrace();
      }
   }
} 
```

 ## 2.struts-config.xml

将 **Hibernate Struts 插件**包含到 Struts 配置文件( **struts-config.xml** )中。

```java
 <struts-config>
    ...
    <plug-in className="com.mkyong.common.plugin.HibernatePlugin">
      	<set-property property="path" value="/hibernate.cfg.xml"/>
    </plug-in>
	...
<struts-config> 
```

 ## 3.获取 Hibernate 会话工厂

在 Struts action 类中，可以从 servlet 上下文中获得 **Hibernate 会话工厂。**

```java
 servlet.getServletContext().getAttribute(HibernatePlugin.KEY_NAME); 
```

并像平常一样执行任何休眠任务。

```java
 package com.mkyong.customer.action;

import java.util.Date;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.beanutils.BeanUtils;
import org.apache.struts.action.Action;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.hibernate.Session;
import org.hibernate.SessionFactory;

import com.mkyong.common.plugin.HibernatePlugin;
import com.mkyong.customer.form.CustomerForm;
import com.mkyong.customer.model.Customer;

public class AddCustomerAction extends Action{

  public ActionForward execute(ActionMapping mapping,ActionForm form,
	HttpServletRequest request,HttpServletResponse response) 
  throws Exception {

        SessionFactory sessionFactory = 
	         (SessionFactory) servlet.getServletContext()
                            .getAttribute(HibernatePlugin.KEY_NAME);

	Session session = sessionFactory.openSession();

	CustomerForm customerForm = (CustomerForm)form;
	Customer customer = new Customer();

	//copy customerform to model
	BeanUtils.copyProperties(customer, customerForm);

	//save it
	customer.setCreatedDate(new Date());

	session.beginTransaction();
	session.save(customer);
	session.getTransaction().commit();

	return mapping.findForward("success");

  }
} 
```

完成了。

[hibernate](http://web.archive.org/web/20190214221344/http://www.mkyong.com/tag/hibernate/) [integration](http://web.archive.org/web/20190214221344/http://www.mkyong.com/tag/integration/) [struts](http://web.archive.org/web/20190214221344/http://www.mkyong.com/tag/struts/)







