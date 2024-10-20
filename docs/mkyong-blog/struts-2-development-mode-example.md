# struts 2–开发模式示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-development-mode-example/>

在 Struts 2 开发中，这应该是要学习的第一个可配置值。为了启用 Struts 2 开发模式，您可以通过给予**自动配置和属性文件重新加载**和**额外的日志记录和调试**特性来显著提高您的 Struts 2 开发速度。

The auto reload feature is really a convenient feature. Each time i made changed in properties or XML configuration file, the application is no longer need to restart to take effect.By default, the Struts 2 development mode is disabled.

## 启用 Strut2 开发模式

在 struts 属性文件或 XML 配置文件中将" **struts.devMode** "值设置为 true。

**struts.properties**

```java
struts.devMode = true

```

**struts.xml**

```java
 <struts>
 	<constant name="struts.devMode" value="true" />	
</struts> 
```

 ## 禁用 Strut2 开发模式

在 struts 属性文件或 XML 配置文件中将" **struts.devMode** "设置为 false。

**struts.properties**

```java
struts.devMode = false

```

**struts.xml**

```java
 <struts>
 	<constant name="struts.devMode" value="false" />	
</struts> 
```

The development mode is only suitable in development or debugging environment. In production environment, **you HAVE TO disabled it**. It will caused significant impact on performance, because the entire application configuration, properties files will be reloaded on every request, many extra logging and debug information will be provide also.Before commit Struts configuration file, just make sure the development mode is turn off. I saw many accidentally commit cases – commit with development mode enable, and someone just grab the source code for QA environment. To be Frankly, QA seldom will do the performance test, they just make sure the functionality are there, and end with a development mode enabled application deployed to the production. Guess what? you will receive many screaming phone calls from clients very soon… ## 参考

1.  [Struts 2 开发模式文档](http://web.archive.org/web/20190225092948/http://struts.apache.org/2.1.8/docs/devmode.html)

[struts2](http://web.archive.org/web/20190225092948/http://www.mkyong.com/tag/struts2/)







