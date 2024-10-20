# 如何在方法表达式中传递参数–JSF 2.0

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-pass-parameters-in-method-expression-jsf-2-0/>

从 JSF 2.0 开始，允许在类似" **#{bean.method(param)}** "的方法表达式中传递参数值，但是这个特性会在 Tomcat 服务器上引发" **EL 解析错误**"。举个例子，

*被管理的 Bean*

```java
 @ManagedBean(name="order")
@SessionScoped
public class OrderBean implements Serializable{

	public String editAction(String id) {
		//...
	}
} 
```

*JSF 页面*

```java
 //...
<h:commandLink value="Edit" action="#{order.editAction(123)}" />
//... 
```

如果部署在 Tomcat 上，它将显示以下错误消息:

```java
 An Error Occurred:
Error Parsing: #{order.editAction(123)} 
```

或者

```java
 javax.el.MethodNotFoundException 
```

## 解决办法

实际上，这个所谓的**方法表达式参数**是`EL 2.2`的一个特性，也就是**在 Tomcat 中默认不支持**。

为了使用这个特性，你必须从 Java.net 的[那里获得“ **el-impl-2.2.jar** ”，并把它放到你的项目依赖文件夹中。](http://web.archive.org/web/20190719060034/http://download.java.net/maven/2/org/glassfish/web/el-impl/2.2/el-impl-2.2.pom)

*文件:pom.xml*

```java
 <dependency>
	  <groupId>org.glassfish.web</groupId>
	  <artifactId>el-impl</artifactId>
	  <version>2.2</version>
     </dependency> 
```

做完了，Tomcat 应该可以支持 JSF 2.0 web 应用中的**方法表达式参数**。

[jsf2](http://web.archive.org/web/20190719060034/https://www.mkyong.com/tag/jsf2/) [parameter](http://web.archive.org/web/20190719060034/https://www.mkyong.com/tag/parameter/)<input type="hidden" id="mkyong-postId" value="7382">







