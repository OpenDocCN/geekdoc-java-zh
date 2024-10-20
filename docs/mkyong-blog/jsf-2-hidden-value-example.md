# JSF 2 隐藏价值示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-hidden-value-example/>

在 JSF，你可以使用 **< h:inputHidden / >** 标签来呈现一个 HTML 隐藏值字段。举个例子，

JSF 标签…

```java
 <h:inputHidden value="some text" /> 
```

呈现此 HTML 代码…

```java
 <input type="hidden" name="random value" value="some text" /> 
```

## JSF 隐藏字段示例

一个 JSF 2 的例子，通过 **< h:inputHidden / >** 标签呈现一个隐藏字段，并在 JavaScript 中访问隐藏值。

## 1.受管 Bean

一个简单的托管 bean，声明为“user”。

```java
 package com.mkyong.form;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import java.io.Serializable;

@ManagedBean(name="user")
@SessionScoped
public class UserBean implements Serializable {

	String answer = "I'm Hidden value!";

	public String getAnswer() {
		return answer;
	}

	public void setAnswer(String answer) {
		this.answer = answer;
	}	
} 
```

## 2.查看页面

通过“h:inputHidden”标签呈现一个隐藏值，如果按钮被点击，通过 JavaScript 打印隐藏值。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html">

	<h:head>
	  <script type="text/javascript">
	    function printHiddenValue(){

	       alert(document.getElementById('myform:hiddenId').value);	

	    }
	  </script>
	</h:head>
    <h:body>
    	<h1>JSF 2 hidden value example</h1>

	  <h:form id="myform">
    		<h:inputHidden value="#{user.answer}" id="hiddenId" />
    		<h:commandButton type="button" value="ClickMe" onclick="printHiddenValue()" />
    	  </h:form>

    </h:body>
</html> 
```

## 3.演示

*URL:http://localhost:8080/Java server faces/*



![jsf2-hidden-value--example-1](img/baf0ffc56fa5fae07810be96fa1027ac.png "jsf2-hidden-value--example-1")

## 下载源代码

Download It – [JSF-2-HiddenValue-Example.zip](http://web.archive.org/web/20201212022659/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-HiddenValue-Example.zip) (9KB)**Note**
You may interest to know how to [pass new hidden value to backing bean in JSF](http://web.archive.org/web/20201212022659/http://www.mkyong.com/jsf2/how-to-pass-new-hidden-value-to-backing-bean-in-jsf/).

#### 参考

1.  [JSF < h:输入隐藏/ > JavaDoc](http://web.archive.org/web/20201212022659/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/h/inputHidden.html)

标签:[隐藏值](http://web.archive.org/web/20201212022659/https://mkyong.com/tag/hidden-value/)[JS F2](http://web.archive.org/web/20201212022659/https://mkyong.com/tag/jsf2/)<input type="hidden" id="mkyong-current-postId" value="7157">

### 相关文章

*   [How to pass the new hidden value to the backing bean 【T1] in JS](/web/20201212022659/https://www.mkyong.com/jsf2/how-to-pass-new-hidden-value-to-backing-bean-in-jsf/)
*   [How to get the hidden field value](/web/20201212022659/https://www.mkyong.com/javascript/how-to-get-hidden-field-value-in-javascript/) in JavaScript
*   [JSF 2.0 tutorial](/web/20201212022659/https://www.mkyong.com/tutorials/jsf-2-0-tutorials/)

*   [ Spring MVC Hidden Value Example
*   [JSF 新协议预发布会示例](/web/20201212022659/https://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)
*   [JSF 2.0](/web/20201212022659/https://www.mkyong.com/jsf2/multi-components-validator-in-jsf-2-0/)
*   [Example of JSF2 multi-choice list box](/web/20201212022659/https://www.mkyong.com/jsf2/jsf-2-multiple-select-listbox-example/)
*   [JSF 2 链接、commandLink 和输出链接示例](/web/20201212022659/https://www.mkyong.com/jsf2/jsf-2-link-commandlink-and-outputlink-example/)