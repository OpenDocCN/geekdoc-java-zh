# 如何加载多个 Spring bean 配置文件

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring/load-multiple-spring-bean-configuration-file/>

## 问题

在大型项目结构中，Spring 的 bean 配置文件位于不同的文件夹中，以便于维护和模块化。例如:常用文件夹中的`Spring-Common.xml`、连接文件夹中的`Spring-Connection.xml`、模块文件夹中的`Spring-ModuleA.xml`等等。

您可以在代码中加载多个 Spring bean 配置文件:

```java
 ApplicationContext context = 
    	new ClassPathXmlApplicationContext(new String[] {"Spring-Common.xml",
              "Spring-Connection.xml","Spring-ModuleA.xml"}); 
```

将所有 spring xml 文件放在项目类路径下。

```java
 project-classpath/Spring-Common.xml
	project-classpath/Spring-Connection.xml
	project-classpath/Spring-ModuleA.xml 
```

 ## 解决办法

上述方法缺乏组织性且容易出错，更好的方法应该是将所有的 Spring bean 配置文件组织到一个 XML 文件中。例如，创建一个`Spring-All-Module.xml`文件，并像这样导入整个 Spring bean 文件:

File : Spring-All-Module.xml

```java
 <beans 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<import resource="common/Spring-Common.xml"/>
        <import resource="connection/Spring-Connection.xml"/>
        <import resource="moduleA/Spring-ModuleA.xml"/>

</beans> 
```

现在，您可以像这样加载一个 xml 文件:

```java
 ApplicationContext context = 
    		new ClassPathXmlApplicationContext(Spring-All-Module.xml); 
```

将该文件放在项目类路径下。

```java
 project-classpath/Spring-All-Module.xml 
```

**Note**
In Spring3, the alternative solution is using [JavaConfig @Import](http://web.archive.org/web/20190225095319/http://www.mkyong.com/spring3/spring-3-javaconfig-import-example/).[spring](http://web.archive.org/web/20190225095319/http://www.mkyong.com/tag/spring/)![](img/caed34cd26b4e2d29ecc458cea8209c9.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225095319/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="3609">







