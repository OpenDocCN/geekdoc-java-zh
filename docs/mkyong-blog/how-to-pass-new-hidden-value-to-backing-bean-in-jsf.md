# 如何将新的隐藏价值传递给 JSF 的 backing bean

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-pass-new-hidden-value-to-backing-bean-in-jsf/>

在某些情况下，您可能需要向后备 bean 传递一个新的隐藏值。一般有两种方式:

## 1.HTML 标记+ getRequestParameterMap()

通过 **getRequestParameterMap()** 方法，使用普通 HTML 输入、硬编码的新隐藏值和后台 bean 中的访问来呈现隐藏字段。

JSF…

```java
 <h:form id="myForm">
    <input type="hidden" name="hidden1" value="this is hidden2" />
    <h:commandButton value="submit" action="#{user.action}" />
</h:form> 
```

受管 bean…

```java
 @ManagedBean(name="user")
@SessionScoped
public class UserBean
{
	public String action(){
	   String value = FacesContext.getCurrentInstance().
		getExternalContext().getRequestParameterMap().get("hidden1");
	}
} 
```

 ## 2.JSF 标签+ JavaScript

通过“h:inputHidden”标记呈现隐藏字段，通过 JavaScript 分配新值。

JSF…

```java
 <script type="text/javascript">
   function setHiddenValue(new_value){

	document.getElementById('myForm:hidden2').value = new_value;

   }
</script>
<h:form id="myForm">		    
   <h:inputHidden id="hidden2" value="#{user.hidden2}" />
   <h:commandButton value="submit" action="..." onclick="setHiddenValue('this is hidden2');" />
</h:form> 
```

受管 bean…

```java
 @ManagedBean(name="user")
@SessionScoped
public class UserBean
{
	public String hidden2;

	public void setHidden2(String hidden2) {
		this.hidden2 = hidden2;
	}
} 
```

 ## JSF 2.0 新隐藏价值示例

一个 JSF 2.0 的例子，演示了使用上述两种方法来传递一个新的隐藏值给一个后台 bean。

## 1.受管 Bean

一个简单的受管 bean，将名称指定为“user”。

```java
 package com.mkyong.form;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.faces.context.FacesContext;

import java.io.Serializable;

@ManagedBean(name="user")
@SessionScoped
public class UserBean implements Serializable {

	public String hidden1;
	public String hidden2;

	public String getHidden2() {
		return hidden2;
	}

	public void setHidden2(String hidden2) {
		this.hidden2 = hidden2;
	}

	public String getHidden1() {
		return hidden1;
	}

	public void setHidden1(String hidden1) {
		this.hidden1 = hidden1;
	}

	public String action(){

	    String value = FacesContext.getCurrentInstance().
		getExternalContext().getRequestParameterMap().get("hidden1");
	    setHidden1(value);

	    return "start";
	}	
} 
```

## 2.查看页面

两页用于演示。

**demo . XHTML**–传递新隐藏值的两种方法。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html">

	<h:head>
	<script type="text/javascript">
	   function setHiddenValue(new_value){

	     document.getElementById('myForm:hidden2').value = new_value;

	   }
	</script>
	</h:head>
    <h:body>
     <h1>JSF 2 pass new hidden value to backing bean</h1>

     <h:form id="myForm">

       <input type="hidden" name="hidden1" value="this is hidden2" />

       <h:inputHidden id="hidden2" value="#{user.hidden2}" />

       <h:commandButton value="submit" action="#{user.action}" 
                               onclick="setHiddenValue('this is hidden2');" />
     </h:form>

    </h:body>
</html> 
```

**start . XHTML**–通过“h:outputText”标签显示隐藏值。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html">

    <h:body>
    	<h1>JSF 2 pass new hidden value to backing bean</h1>
 	<ol>
 	  <li>Hidden1 = <h:outputText value="#{user.hidden1}" /></li>
 	  <li>Hidden2 = <h:outputText value="#{user.hidden2}" /></li>
	</ol>
    </h:body>
</html> 
```

## 3.演示

*URL:http://localhost:8080/Java server faces/*

![jsf2-new-hidden-example-1](img/320c2c08cf153ecc4b7ddd37ecd1c9de.png "jsf2-new-hidden-example-1")![jsf2-new-hidden-example-2](img/82d889029c6f2e2848f9c1cb7cc3d18f.png "jsf2-new-hidden-example-2")

## 下载源代码

Download It – [JSF-2-New-HiddenValue-Example.zip](http://web.archive.org/web/20190306165414/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-New-HiddenValue-Example.zip) (10KB)

#### 参考

1.  [JSF < h:输入隐藏/ > JavaDoc](http://web.archive.org/web/20190306165414/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/h/inputHidden.html)

[靠山豆](http://web.archive.org/web/20190306165414/http://www.mkyong.com/tag/backing-bean/) [隐藏值](http://web.archive.org/web/20190306165414/http://www.mkyong.com/tag/hidden-value/)[JSF 2](http://web.archive.org/web/20190306165414/http://www.mkyong.com/tag/jsf2/)







