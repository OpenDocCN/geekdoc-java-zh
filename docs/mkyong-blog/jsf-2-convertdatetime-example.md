# JSF 新协议转换日期示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-convertdatetime-example/>

**f:convertDateTime** "是一个标准的 JSF 转换器标签，它将字符串转换成指定的“日期”格式。此外，它还用于实现日期验证。

下面的 JSF 2.0 示例向您展示了如何使用这个" **f:convertDateTime** "标记。

## 1.受管 Bean

一个简单的托管 bean，具有“日期”属性。

```java
 package com.mkyong;

import java.io.Serializable;
import java.util.Date;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="receipt")
@SessionScoped
public class ReceiptBean implements Serializable{

	Date date;

	public Date getDate() {
		return date;
	}

	public void setDate(Date date) {
		this.date = date;
	}

} 
```

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 2.f:convertDateTime 示例

用“ **f:convertDateTime** 标签实现日期验证。接受的日期格式在“**模式**属性中定义。

**Note**
The date format in “pattern” attribute is defined in the [java.text.SimpleDateFormat](http://web.archive.org/web/20210220021019/https://download.oracle.com/javase/1.5.0/docs/api/java/text/SimpleDateFormat.html).

**default.xhtml**

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

    	<h1>JSF 2 convertDate example</h1>

	<h:form>

		<h:panelGrid columns="3">

		  Receipt Date : 
		 <h:inputText id="date" value="#{receipt.date}" 
			size="20" required="true"
			label="Receipt Date" >

			<f:convertDateTime pattern="d-M-yyyy" />
		  </h:inputText>

		  <h:message for="date" style="color:red" />

		</h:panelGrid>

		<h:commandButton value="Submit" action="receipt" />

	   </h:form>

    </h:body>
</html> 
```

**receipt.xhtml**

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

    	<h1>JSF 2 convertDate example</h1>

		Receipt Date :  
		<h:outputText value="#{receipt.date}" >
			<f:convertDateTime pattern="d-M-yyyy" />
		</h:outputText>

    </h:body>
</html> 
```

## 3.演示

如果提供的日期无效，则显示错误消息。



![jsf2-ConvertDateTime-Example](img/7ef9e499477360e371f8aa4bc43dea54.png "jsf2-ConvertDateTime-Example")

## 下载源代码

Download It – [JSF-2-ConvertDateTime-Example.zip](http://web.archive.org/web/20210220021019/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-ConvertDateTime-Example.zip) (10KB)

## 参考

1.  [JSF 2 号 convertDateTime JavaDoc](http://web.archive.org/web/20210220021019/http://javaserverfaces.java.net/nonav/docs/2.0/pdldocs/facelets/f/convertDateTime.html)
2.  [简单日期格式 JavaDoc](http://web.archive.org/web/20210220021019/https://download.oracle.com/javase/1.5.0/docs/api/java/text/SimpleDateFormat.html)

Tags : [date](http://web.archive.org/web/20210220021019/https://mkyong.com/tag/date/) [jsf2](http://web.archive.org/web/20210220021019/https://mkyong.com/tag/jsf2/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="7468">