# JSF 2 有效双程示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-validatedoublerange-example/>

" **f:validateDoubleRange** "是一个 JSF 范围验证器标签，用于验证浮点值的范围。举个例子，

```java
 <h:inputText id="salary" value="#{user.salary}">
	<f:validateDoubleRange minimum="10.11" maximum="10000.11" />
</h:inputText> 
```

提交该表单时，验证器将确保“薪金”值在“10.11”到“10000.11”的范围内。

## “f:validateDoubleRange”示例

一个 JSF 2.0 的例子展示了如何使用" **f:validateDoubleRange** "标签来验证" salary "输入字段的范围，当验证器失败时，通过" **h:message** "标签显示错误消息。

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 1.受管 Bean

用户管理的 bean，具有“薪金”属性。

```java
 package com.mkyong;

import java.io.Serializable;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="user")
@SessionScoped
public class UserBean implements Serializable{

	double salary;

	public double getSalary() {
		return salary;
	}

	public void setSalary(double salary) {
		this.salary = salary;
	}

} 
```

## 2.JSF·佩奇

JSF XHTML 页面，展示了如何使用" **f:validateDoubleRange** "标记来确保" salary "输入值在" 10.11 "到" 10000.11 "的范围内。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >
    <h:body>

    	<h1>JSF 2 validateDoubleRange example</h1>

	<h:form>

		<h:panelGrid columns="3">

			Enter your salary : 

			<h:inputText id="salary" value="#{user.salary}" 
				size="10" required="true"
				label="Salary" >

				<f:validateDoubleRange minimum="10.11" maximum="10000.11" />

			</h:inputText>

			<h:message for="salary" style="color:red" />

		</h:panelGrid>

		<h:commandButton value="Submit" action="result" />

	</h:form>

    </h:body>
</html> 
```

## 3.演示

最小范围验证失败。



![jsf2-ValidateDoubleRange-Example-1](img/c025eb7d3a624ac83c68dac85ec516a5.png "jsf2-ValidateDoubleRange-Example-1")

## 下载源代码

Download It – [JSF-2-ValidateDoubleRange-Example.zip](http://web.archive.org/web/20210220021049/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-ValidateDoubleRange-Example.zip) (9KB)

## 参考

1.  [JSF 2 验证了 DoubleRange JavaDoc](http://web.archive.org/web/20210220021049/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/f/validateDoubleRange.html)
2.  [维基浮点解释](http://web.archive.org/web/20210220021049/https://en.wikipedia.org/wiki/Floating_point)

Tags : [jsf2](http://web.archive.org/web/20210220021049/https://mkyong.com/tag/jsf2/) [validation](http://web.archive.org/web/20210220021049/https://mkyong.com/tag/validation/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="7513">