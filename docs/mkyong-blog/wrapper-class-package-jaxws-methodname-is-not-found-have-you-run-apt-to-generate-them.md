# 找不到包装类 package.jaxws.methodName。你有没有倾向于生成它们？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/webservices/jax-ws/wrapper-class-package-jaxws-methodname-is-not-found-have-you-run-apt-to-generate-them/>

## 问题

在 JAX-WS 开发中，当部署以下服务端点时，

*文件:HelloWorld.java*

```java
 package com.mkyong.ws;
//Service Endpoint Interface
@WebService
public interface HelloWorld{

	@WebMethod String getHelloWorldAsString();
} 
```

*文件:HelloWorldImpl.java*

```java
 //Service Implementation
package com.mkyong.ws;
@WebService(endpointInterface = "com.mkyong.ws.HelloWorld")
public class HelloWorldImpl implements HelloWorld{

	@Override
	public String getHelloWorldAsString() {
		//...
	}

} 
```

它会立即显示以下错误信息？

```java
 Exception in thread "main" com.sun.xml.internal.ws.model.RuntimeModelerException: 
	runtime modeler error: 

        Wrapper class com.mkyong.ws.jaxws.GetHelloWorldAsString is not found. 
        Have you run APT to generate them?

	at com.sun.xml.internal.ws.model.RuntimeModeler.getClass(RuntimeModeler.java:256)
	//... 
```

 ## 解决办法

服务端点接口没有用任何`@SOAPBinding`进行注释，因此，它使用默认的**文档样式**来发布它。为了便于阅读，您可以将其重写如下:

```java
 //Service Endpoint Interface
@WebService
@SOAPBinding(style = Style.DOCUMENT, use=Use.LITERAL)
public interface HelloWorld{

	@WebMethod String getHelloWorldAsString();
} 
```

在文档风格中，您需要使用" **wsgen** "工具来为服务发布生成所有必要的 JAX-WS 可移植工件(映射类、wsdl 或 xsd 模式)。

 ## wsgen 命令

读取服务端点实现类需要使用 **wsgen** 命令:

```java
 wsgen -keep -cp . com.mkyong.ws.HelloWorldImpl 
```

它在 **package.jaxws** 文件夹下为单个`getHelloWorldAsString()`方法生成两个类。

1.  gethelloworldasstring.java
2.  gethelloworlandstrings response . Java

将这些类复制到正确的文件夹中，在本例中是" **com.mkyong.ws.jaxws** "。请尝试再次发布它。

## 参考

1.  [wsgen 工具文档](http://web.archive.org/web/20190308062457/http://download.oracle.com/javase/6/docs/technotes/tools/share/wsgen.html)

[jax-ws](http://web.archive.org/web/20190308062457/http://www.mkyong.com/tag/jax-ws/) [web services](http://web.archive.org/web/20190308062457/http://www.mkyong.com/tag/web-services/)







