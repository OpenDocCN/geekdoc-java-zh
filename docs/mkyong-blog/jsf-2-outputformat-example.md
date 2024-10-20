# JSF 2 输出格式示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-outputformat-example/>

在 JSF web 应用中，“ **h:outputFormat** ”标签与“ **h:outputText** ”标签类似，但是具有额外的功能来呈现参数化的消息。举个例子，

```java
 <h:outputFormat value="param0 : {0}, param1 : {1}" >
 	<f:param value="Number 1" />
 	<f:param value="Number 2" />
</h:outputFormat> 
```

它将输出以下结果

```java
 param0 : Number 1, param1 : Number 2 
```

1.  {0}匹配到<param value="”Number">
2.  {1}与<param value="”Number">匹配

## OutputFormat 示例

查看 JSF 2.0 web 应用中编码的“ **h:outputFormat** ”标签的几个用例。

 ## 1.受管 Bean

一个受管 bean，提供一些文本用于演示。

```java
 import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

@ManagedBean(name="user")
@SessionScoped
public class UserBean{

	public String text = "Hello {0}";
	public String htmlInput = "<input type=\"{0}\" {1} />";

	//getter and setter methods...
} 
```

 ## 2.查看页面

带有几个“ **h:outputFormat** ”标签的页面示例。

JSF…

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      >
    <h:body>
      <h1>JSF 2.0 h:outputFormat Example</h1>
      <ol>
    	<li>
    	  <h:outputFormat value="this is param 0 : {0}, param 1 : {1}" >
 		<f:param value="Number 1" />
 		<f:param value="Number 2" />
 	  </h:outputFormat>
 	</li>
 	<li>
 	  <h:outputFormat value="#{user.text}" >
 		<f:param value="mkyong" />
 	  </h:outputFormat>
 	</li>
	<li>
	  <h:outputFormat value="#{user.htmlInput}" >
 		<f:param value="text" />
 		<f:param value="size='30'" />
 	  </h:outputFormat>
	 </li>
	 <li>
	  <h:outputFormat value="#{user.htmlInput}" escape="false" >
 		<f:param value="text" />
 		<f:param value="size='30'" />
 	  </h:outputFormat>
	 </li>
	 <li>
	  <h:outputFormat value="#{user.htmlInput}" escape="false" >
 		<f:param value="button" />
 		<f:param value="value='Click Me'" />
 	  </h:outputFormat>
	 </li>
       </ol>
    </h:body>
</html> 
```

生成以下 HTML 代码…

```java
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html >
  <body> 
    <h1>JSF 2.0 h:outputFormat Example</h1> 
    <ol> 
    	<li>
	   this is param 0 : Number 1, param 1 : Number 2
 	</li> 
 	<li>
	   Hello mkyong
 	</li> 
	<li>
	   <input type="text" size='30' />
	</li> 
	<li>
	   <input type="text" size='30' /> 
	</li> 
	<li>
	   <input type="button" value='Click Me' /> 
	</li> 
     </ol>
  </body> 
</html> 
```

## 3.演示

*URL:http://localhost:8080/Java server faces/*

![jsf2-outputformat-example](img/a7e74996a63ff5f7d66ce947234dcda7.png "jsf2-outputformat-example")

## 下载源代码

Download It – [JSF-2-OutputFormat-Example.zip](http://web.archive.org/web/20190225100232/http://www.mkyong.com/wp-content/uploads/2010/09/JSF-2-OutputFormat-Example.zip) (9KB)

#### 参考

1.  [JSF < h:输出格式/ > JavaDoc](http://web.archive.org/web/20190225100232/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/h/outputFormat.html)

[JSF 2](http://web.archive.org/web/20190225100232/http://www.mkyong.com/tag/jsf2/)







