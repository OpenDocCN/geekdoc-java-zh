# JSF 2 预渲染事件示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/>

在 JSF 2.0 中，您可以在显示视图根(JSF 页面)之前附加一个`javax.faces.event.PreRenderViewEvent`系统事件来执行定制任务。

让我们看下面一个完整的`PreRenderViewEvent`例子:

## 1.受管 Bean

创建一个普通 bean，包含一个方法签名"**public void method-name(ComponentSystemEvent event)**"，稍后你会要求监听器调用这个方法。

在此方法中，它验证当前会话中的“*角色*，如果角色不等于“*管理员*，则导航到结果“*拒绝访问*”。

```java
 package com.mkyong;

import javax.faces.application.ConfigurableNavigationHandler;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.faces.context.FacesContext;
import javax.faces.event.ComponentSystemEvent;

@ManagedBean(name="user")
@SessionScoped
public class UserBean{

  public void isAdmin(ComponentSystemEvent event){

	FacesContext fc = FacesContext.getCurrentInstance();

	if (!"admin".equals(fc.getExternalContext().getSessionMap().get("role"))){

		ConfigurableNavigationHandler nav 
		   = (ConfigurableNavigationHandler) 
			fc.getApplication().getNavigationHandler();

		nav.performNavigation("access-denied");
	}		
  }	
} 
```

## 2.JSF·佩奇

现在，您使用`f:event`标记将“`preRenderView`”系统事件附加到“default.xhtml”页面。

*default.xhtml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >

 	<f:event listener="#{user.isAdmin}" type="preRenderView" />

	<h:body>

	    <h1>JSF 2 protected page example</h1>

	</h:body>

</html> 
```

*拒绝访问. xhtml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      >

    <h:body>

    	<h1>Access Denied!</h1>

    </h:body>

</html> 
```

## 3.演示

访问这个页面“ *default.xhtml* ”，由于 session 对象中没有“*角色*”值，所以 JSF 会导航到另一个页面“ *access-denied.xhtml* ”。

![jsf2-PreRenderViewEvent-example](img/3b03c580ff5be50785571e1d64f4f5c8.png "jsf2-PreRenderViewEvent-example")

## 下载源代码

Download It – [JSF-2-PreRenderViewEvent-Example.zip](http://web.archive.org/web/20210514070448/http://www.mkyong.com/wp-content/uploads/2010/11/JSF-2-PreRenderViewEvent-Example.zip) (10KB)

## 参考

1.  [JSF 2 预渲染事件 JavaDoc](http://web.archive.org/web/20210514070448/https://javaserverfaces.dev.java.net/nonav/docs/2.0/javadocs/javax/faces/event/PreRenderViewEvent.html)

Tags : [jsf2](http://web.archive.org/web/20210514070448/https://mkyong.com/tag/jsf2/)<input type="hidden" id="mkyong-current-postId" value="7682">