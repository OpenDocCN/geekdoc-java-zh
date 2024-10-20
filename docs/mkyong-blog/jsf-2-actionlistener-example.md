# JSF 2 actionListener 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-actionlistener-example/>

在 JSF，**动作事件**是通过点击按钮或链接组件来触发的，例如 *h:commandButton* 或 *h:commandLink* 。

**actions vs action listeners**
Do not confuse these two tags, **actions** is used to perform business logic and navigation task; While **action listeners** are used to perform UI interface logic or action invoke observation.

这个动作监听器的常见用例是用来取回附加到组件的属性值，参见这个 [JSF 2 f:属性示例](http://web.archive.org/web/20221209075023/http://www.mkyong.com/jsf2/jsf-2-attribute-example/)。

这里有两种方法来实现它:

## 1.方法绑定

在按钮或链接组件中，可以直接在“ **actionListener** ”属性中指定 bean 的方法。

*JSF 页……*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >

    <h:body>

    <h1>JSF 2 actionListener example</h1>

	<h:form id="form">

	  <h:commandButton id="submitButton" 
		value="Submit" action="#{normal.outcome}" 
		actionListener="#{normal.printIt}" />

	</h:form>

    </h:body>
</html> 
```

*托管 Bean…*
与动作事件交互的方法应该接受一个**动作事件**参数。

```java
 package com.mkyong;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.faces.event.ActionEvent;

@ManagedBean(name="normal")
@SessionScoped
public class NormalBean{

	public String buttonId; 

	public void printIt(ActionEvent event){

		//Get submit button id
		buttonId = event.getComponent().getClientId();

	}

	public String outcome(){
		return "result";
	}
} 
```

## 2.动作监听器

在按钮或链接组件中，内部添加一个“ **f:actionListener** 标签，并指定一个 **ActionListener** 接口的实现类，并覆盖其 **processAction()** 。

*JSF 页……*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >

    <h:body>

    <h1>JSF 2 actionListener example</h1>

	<h:form id="form">

	  <h:commandButton id="submitButton" 
		value="Submit" action="#{normal.outcome}" >
		<f:actionListener type="com.mkyong.NormalActionListener" />
	  </h:commandButton>

	</h:form>

    </h:body>
</html> 
```

*action listener 接口的实现*

```java
 package com.mkyong;

import javax.faces.event.AbortProcessingException;
import javax.faces.event.ActionEvent;
import javax.faces.event.ActionListener;

public class NormalActionListener implements ActionListener{

	@Override
	public void processAction(ActionEvent event)
		throws AbortProcessingException {

		System.out.println("Any use case here?");

	}

} 
```

## 下载源代码

Download It – [JSF-2-ActionListener-Example.zip](http://web.archive.org/web/20221209075023/http://www.mkyong.com/wp-content/uploads/2010/11/JSF-2-ActionListener-Example.zip) (10KB)

## 参考

1.  [stack overflow–action 和 actionlistener](http://web.archive.org/web/20221209075023/https://stackoverflow.com/questions/3888710/jsf-1-2-difference-between-exception-in-action-and-actionlistener)
2.  [动作事件 JavaDoc](http://web.archive.org/web/20221209075023/https://javaserverfaces.dev.java.net/nonav/docs/2.0/javadocs/javax/faces/event/ActionEvent.html)

<input type="hidden" id="mkyong-current-postId" value="7625">