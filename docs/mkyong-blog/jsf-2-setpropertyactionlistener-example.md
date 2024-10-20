# JSF 2 setPropertyActionListener 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-setpropertyactionlistener-example/>

在 JSF，“**f:setPropertyActionListener**”标签允许你直接设置一个值到你的后台 bean 的属性中。举个例子，

```java
 <h:commandButton action="#{user.outcome}" value="Submit">
    <f:setPropertyActionListener target="#{user.username}" value="mkyong" />
</h:commandButton> 
```

在上面的 JSF 代码片段中，如果单击按钮，它会通过 *setUsername()* 方法将“ *mkyong* ”值设置为“*用户名*”属性。

```java
 @ManagedBean(name="user")
@SessionScoped
public class UserBean{

	public String username;

	public void setUsername(String username) {
		this.username = username;
	}

} 
```

## JSF f:setPropertyActionListener 示例

好了，让我们来看一个 JSF 2.0 的完整例子。

 ## 1.受管 Bean

一个名为“user”的超级简单的托管 bean。

```java
 package com.mkyong;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="user")
@SessionScoped
public class UserBean{

	public String username;

	public String outcome(){
		return "result";
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

} 
```

 ## 2.JSF·佩奇

JSF 页面展示了如何使用"**f:setPropertyActionListener**"将一个值" *mkyong* "直接设置到您的后台 bean 的属性" *username* "中。

*default.xhtml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >

    <h:body>

    	<h1>JSF 2 setPropertyActionListener example</h1>

	<h:form id="form">

	  <h:commandButton action="#{user.outcome}" value="Click Me">

	      <f:setPropertyActionListener target="#{user.username}" value="mkyong" />

	  </h:commandButton>

	</h:form>

    </h:body>
</html> 
```

*result.xhtml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      >

    <h:body>

    	<h1>JSF 2 setPropertyActionListener example</h1>

	#{user.username}

    </h:body>

</html> 
```

## 3.演示

这是点击按钮后的结果。

![jsf2-setPropertyActionListener-example](img/8be29eb7175af03e1b88250552d8f2b7.png "jsf2-setPropertyActionListener-example")

## 下载源代码

Download It – [JSF-2-setPropertyActionListener-Example.zip](http://web.archive.org/web/20190212075415/http://www.mkyong.com/wp-content/uploads/2010/11/JSF-2-setPropertyActionListener-Example.zip) (9KB)

## 参考

1.  [JSF 2 setPropertyActionListener JavaDoc](http://web.archive.org/web/20190212075415/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/f/setPropertyActionListener.html)

[jsf2](http://web.archive.org/web/20190212075415/http://www.mkyong.com/tag/jsf2/)







