# JSF 2 有效长度示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-validatelength-example/>

" **f:validateLength** "是一个 JSF 字符串长度验证器标签，用于检查字符串的长度。举个例子，

```java
 <h:inputText id="username" value="#{user.username}">
	<f:validateLength minimum="5" maximum="10" />
</h:inputText> 
```

提交表单时，验证器将确保“用户名”文本字段包含的最小长度为 5，最大长度为 10。

## “f:validateLength”示例

一个 JSF 2.0 的例子，展示了使用" **f:validateLength** "标签来验证"用户名"文本字段的长度，当验证器失败时，通过" **h:message** "标签显示错误消息。

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 1.受管 Bean

仅保存“用户名”属性的虚拟受管 bean。

```java
 package com.mkyong;

import java.io.Serializable;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="user")
@SessionScoped
public class UserBean implements Serializable{

	String username;

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

} 
```

## 2.JSF·佩奇

JSF XHTML 页面，展示了如何使用" **f:validateLength** "标记来确保表单的输入" username "包含的最小长度为 5，最大长度为 10。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      xmlns:c="http://java.sun.com/jsp/jstl/core"
      >
    <h:body>

    	<h1>JSF 2 validateLength example</h1>

	<h:form>

		<h:panelGrid columns="3">

			Enter UserName : 

			<h:inputText id="username" value="#{user.username}" 
				size="20" required="true"
				label="UserName" >
				<f:validateLength minimum="5" maximum="10" />
			</h:inputText>

			<h:message for="username" style="color:red" />

		</h:panelGrid>

		<h:commandButton value="Submit" action="result" />

	</h:form>	
    </h:body>
</html> 
```

## 3.演示

最小长度验证失败。



![jsf2-ValidateLength-Example-1](img/3191d08892236a996d6bb6ce27f4e9a8.png "jsf2-ValidateLength-Example-1")

最大长度验证失败。



![jsf2-ValidateLength-Example-2](img/ba4bcac7d2d730a4647cf182997b2790.png "jsf2-ValidateLength-Example-2")

## 下载源代码

Download It – [JSF-2-ValidateLength-Example.zip](http://web.archive.org/web/20210220021031/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-ValidateLength-Example.zip) (9KB)

## 参考

1.  [JSF 2 有效长度 JavaDoc](http://web.archive.org/web/20210220021031/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/f/validateLength.html)

Tags : [jsf2](http://web.archive.org/web/20210220021031/https://mkyong.com/tag/jsf2/) [validatation](http://web.archive.org/web/20210220021031/https://mkyong.com/tag/validatation/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="7496">