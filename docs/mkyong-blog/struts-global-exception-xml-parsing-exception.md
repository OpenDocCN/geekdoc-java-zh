# struts–<global-exceptions>XML 解析异常</global-exceptions>

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/struts-global-exception-xml-parsing-exception/>

## 问题

struts-config.xml 中的一个 **<全局异常>** 异常处理程序示例

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<global-exceptions>
	    <exception
	      key="error.io.key"
	      type="java.io.IOException"
	      path="/error.jsp" />
	</global-exceptions>

	<form-beans>
		<form-bean name="dynaUserForm"   
		      type="org.apache.struts.action.DynaActionForm">
		      <form-property name="username" type="java.lang.String"/>
		</form-bean>
	</form-beans>

	<action-mappings>
	    //...
	</action-mappings>

</struts-config> 
```

部署过程中，提示**org . Apache . commons . digester . digester**错误。

```java
 19 April 2010 6:50:13 PM org.apache.commons.digester.Digester error
SEVERE: Parse Error at line 52 column 17: The content of element type 
"struts-config" must match "(display-name?,description?,form-beans?,
global-exceptions?,global-forwards?,action-mappings?,controller?,
message-resources*,plug-in*)". 
```

**<全局异常>** 语法绝对正确，你发现错误了吗？

 ## 解决办法

struts-config.xml " **的元素必须以正确的顺序出现。再次查看错误消息:**

```java
 "struts-config" must match 
display-name?,
description?,
form-beans?,
global-exceptions?,
global-forwards?,
action-mappings?,
controller?,
message-resources*,
plug-in* 
```

**<表单-bean>**必须出现在 **<全局-异常>** 之前。您需要将 struts-config.xml 更改为以下顺序:

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC 
"-//Apache Software Foundation//DTD Struts Configuration 1.3//EN" 
"http://jakarta.apache.org/struts/dtds/struts-config_1_3.dtd">

<struts-config>

	<form-beans>
		<form-bean name="dynaUserForm"   
		      type="org.apache.struts.action.DynaActionForm">
		      <form-property name="username" type="java.lang.String"/>
		</form-bean>
	</form-beans>

        <global-exceptions>
	    <exception
	      key="error.io.key"
	      type="java.io.IOException"
	      path="/error.jsp" />
	</global-exceptions>

	<action-mappings>
	    //...
	</action-mappings>

</struts-config> 
```

[struts](http://web.archive.org/web/20190209023104/http://www.mkyong.com/tag/struts/)![](img/8222687a7641214b77ea35bd12c3530e.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190209023104/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4606">







