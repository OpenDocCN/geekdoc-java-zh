# spring propertyplaceholderconfigure 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/spring-propertyplaceholderconfigurer-example/>

通常，大多数 Spring 开发人员只是将整个部署细节(数据库细节、日志文件路径)放在 XML bean 配置文件中，如下所示:

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="customerDAO" class="com.mkyong.customer.dao.impl.JdbcCustomerDAO">

		<property name="dataSource" ref="dataSource" />
	</bean>

	<bean id="customerSimpleDAO" class="com.mkyong.customer.dao.impl.SimpleJdbcCustomerDAO">

		<property name="dataSource" ref="dataSource" />
	</bean>

	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">

		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/mkyongjava" />
		<property name="username" value="root" />
		<property name="password" value="password" />
	</bean>

</beans> 
```

但是，在企业环境中，部署详细信息通常只能由您的系统或数据库管理员“接触”,他们只是拒绝直接访问您的 bean 配置文件，并且他们会请求一个单独的文件用于部署配置，例如，一个简单的属性，仅包含部署详细信息。

## PropertyPlaceholderConfigurer 示例

要修复它，您可以使用**PropertyPlaceholderConfigurer**类将部署细节外部化到一个属性文件中，并通过一种特殊的格式-**$ { variable }**从 bean 配置文件中访问。

创建一个属性文件(database.properties)，包含您的数据库详细信息，将其放入您的项目类路径中。

```java
 jdbc.driverClassName=com.mysql.jdbc.Driver
	jdbc.url=jdbc:mysql://localhost:3306/mkyongjava
	jdbc.username=root
	jdbc.password=password 
```

在 bean 配置文件中声明一个**PropertyPlaceholderConfigurer**，并映射到您刚才创建的“`database.properties`”属性文件。

```java
 <bean 
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">

		<property name="location">
			<value>database.properties</value>
		</property>
	</bean> 
```

完整示例

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">

		<property name="location">
			<value>database.properties</value>
		</property>
	</bean>

	<bean id="customerDAO" class="com.mkyong.customer.dao.impl.JdbcCustomerDAO">

		<property name="dataSource" ref="dataSource" />
	</bean>

	<bean id="customerSimpleDAO" 
                class="com.mkyong.customer.dao.impl.SimpleJdbcCustomerDAO">

		<property name="dataSource" ref="dataSource" />
	</bean>

	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">

		<property name="driverClassName" value="${jdbc.driverClassName}" />
		<property name="url" value="${jdbc.url}" />
		<property name="username" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
	</bean>

</beans> 
```

**Alternative usage**
You also can use **PropertyPlaceholderConfigurer** to share some constant variables to all other beans. For example, define your log file location in a properties file, and access the properties value from different beans configuration files via `${log.filepath}`. ## 下载源代码

Download it – [Spring-JDBC-PropertyPlaceholderConfigurer-Example.zip](http://web.archive.org/web/20190210020435/http://www.mkyong.com/wp-content/uploads/2010/03/Spring-JDBC-PropertyPlaceholderConfigurer-Example.zip)[spring](http://web.archive.org/web/20190210020435/http://www.mkyong.com/tag/spring/)![](img/5178a31d36b3e4d9f891ea7430dc4f57.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190210020435/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3910">







