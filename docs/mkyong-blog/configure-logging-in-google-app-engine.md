# 在 Google App Engine 中配置日志记录

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/configure-logging-in-google-app-engine/>

Google App Engine for Java 正在使用 [java.util.logging.Logger](http://web.archive.org/web/20190303001942/http://docs.oracle.com/javase/6/docs/api/java/util/logging/Logger.html) 来执行日志记录。在本教程中，我们将向您展示如何在 GAE 环境中进行日志记录。

## 1.日志示例

在`MovieController`中定义一个记录器，并记录不同记录级别(信息、警告和错误)的消息的示例

```java
 import java.util.logging.Logger;
//...

@Controller
@RequestMapping("/movie")
public class MovieController {

	private static final Logger log = Logger.getLogger(MovieController.class.getName());

	@RequestMapping(value="/{name}", method = RequestMethod.GET)
	public String getMovie(@PathVariable String name, ModelMap model) {

	log.info("Information log message.");

        log.warning("Warning log message.");

        log.severe("Error log message.");

	return "page";

	}

} 
```

 ## 2.日志示例

创建一个文件 logging.properties，设置日志级别，并将其放入" **$project/war/WEB-INF** "

*文件:$ project/war/we b-INF/logging . properties*

```java
 # Set the default logging level for all loggers to WARNING
.level = WARNING 
```

 ## 3.appengine-web.xml

更新 **appengine-web.xml** ，定义系统属性标签，将日志指向上面的“**we b-INF/logging . properties**”。

*文件:$ project/war/we b-INF/app engine-WEB . XML*

```java
 <?xml version="1.0" encoding="utf-8"?>
<appengine-web-app >
  <application>mkyong-springmvc</application>
  <version>1</version>

  <!-- Configure java.util.logging -->
  <system-properties>
    <property name="java.util.logging.config.file" value="WEB-INF/logging.properties"/>
  </system-properties>

</appengine-web-app> 
```

## 4.完成的

在 GAE 本地开发环境中，所有记录的消息都将显示在控制台上。在 GAE 生产环境中，您可以在应用程序的管理员页面中访问记录的消息。

![gae java logging](img/200094e9347ebb4e26caf80601a7d2d4.png "gae-logging-example")

## 参考

1.  [java.util.logging.Logger](http://web.archive.org/web/20190303001942/http://docs.oracle.com/javase/6/docs/api/java/util/logging/Logger.html)
2.  [GAE 测井](http://web.archive.org/web/20190303001942/https://developers.google.com/appengine/docs/java/runtime#Logging)

[gae](http://web.archive.org/web/20190303001942/http://www.mkyong.com/tag/gae/) [logs](http://web.archive.org/web/20190303001942/http://www.mkyong.com/tag/logs/)







