# Java . lang . illegalargumentexception:javax . faces . context . exception handlerfactory

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/java-lang-illegalargumentexception-javax-faces-context-exceptionhandlerfactory/>

## 问题

在 Eclipse IDE 中，将 JSF 2.0 web 应用程序部署到 Tomcat 6.0.26 时，遇到以下异常，无法启动 Tomcat 服务器。

*P.S 两个 **jsf-api-2.1.0-b03.jar** 和 **jsf-impl-2.1.0-b03.jar** 库都包含在项目类路径中。*

```java
 INFO: Unsanitized stacktrace from failed start...
java.lang.IllegalArgumentException: javax.faces.context.ExceptionHandlerFactory
	at javax.faces.FactoryFinder.validateFactoryName(FactoryFinder.java:630)
	at javax.faces.FactoryFinder.setFactory(FactoryFinder.java:287)
	...
SEVERE: Critical error during deployment: 
com.sun.faces.config.ConfigurationException: 
CONFIGURATION FAILED! javax.faces.context.ExceptionHandlerFactory
	...
Caused by: java.lang.IllegalArgumentException: javax.faces.context.ExceptionHandlerFactory
	at javax.faces.FactoryFinder.validateFactoryName(FactoryFinder.java:630)
	... 
```

 ## 解决办法

在 Eclipse 调试模式下，深入源代码内部，找出 FactoryFinder 的 **getFactory()** 方法抛出的 **IllegalArgumentException** 。

**FactoryFinder.java**

```java
 /*
 * @throws IllegalArgumentException if <code>factoryName</code> does not
 *                                  identify a standard JavaServer Faces factory name
 * @throws IllegalStateException    if there is no configured factory
 *                                  implementation class for the specified factory name
 * @throws NullPointerException     if <code>factoryname</code>
 *                                  is null
 */

 public static Object getFactory(String factoryName)
         throws FacesException {

        validateFactoryName(factoryName);
        //...
 } 
```

IllegalArgumentException 记录了因子查找器无法识别新的 JSF 2.0**exception handler factory**工厂名称。

经过上千次的测试和尝试，我终于找到了问题的根源，那就是我的项目类路径中包含的 **javaee.jar** 。看看 **javaee.jar** 里面，它也包含了一套 JSF 1.2 api，看起来 Tomcat 选择了这个 JSF 1.2 api，而不是我的新的 JSF 2.0 api。

在从项目类路径中移除 **javaee.jar** 之后，JSF 2.0 web 应用程序能够在 Tomcat 上启动并运行良好。

 ## 参考

1.  [factory finder . html # get factory Javadoc](http://web.archive.org/web/20190228163119/http://download.oracle.com/docs/cd/E17802_01/j2ee/javaee/javaserverfaces/2.0/docs/api/javax/faces/FactoryFinder.html#getFactory%28java.lang.String%29)
2.  [JSF 2.0.3 发行说明](http://web.archive.org/web/20190228163119/https://javaserverfaces.dev.java.net/nonav/rlnotes/2.0.3/releasenotes.html)

[jsf2](http://web.archive.org/web/20190228163119/http://www.mkyong.com/tag/jsf2/)







