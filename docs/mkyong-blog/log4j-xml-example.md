# log4j.xml 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/logging/log4j-xml-example/>

下面是 log4j 属性文件的 XML 版本，仅供分享。

## 1.输出到控制台

将日志记录重定向到控制台。

log4j.xml

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true"
	xmlns:log4j='http://jakarta.apache.org/log4j/'>

	<appender name="console" class="org.apache.log4j.ConsoleAppender">
	    <layout class="org.apache.log4j.PatternLayout">
		<param name="ConversionPattern" 
		  value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
	    </layout>
	</appender>

	<root>
		<level value="DEBUG" />
		<appender-ref ref="console" />
	</root>

</log4j:configuration> 
```

## 2.输出到文件

将日志记录重定向到文件..

log4j.xml

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true"
	xmlns:log4j='http://jakarta.apache.org/log4j/'>

	<appender name="file" class="org.apache.log4j.RollingFileAppender">
	   <param name="append" value="false" />
	   <param name="maxFileSize" value="10KB" />
	   <param name="maxBackupIndex" value="5" />
	   <!-- For Tomcat -->
	   <param name="file" value="${catalina.home}/logs/myStruts1App.log" />
	   <layout class="org.apache.log4j.PatternLayout">
		<param name="ConversionPattern" 
			value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
	   </layout>
	</appender>

	<root>
		<level value="ERROR" />
		<appender-ref ref="file" />
	</root>

</log4j:configuration> 
```

log4j 滚动文件的例子。

![struts1-log4j-file](img/7c03d5194e835dab0e5062267a04fff0.png)

## 3.输出到控制台和文件

将日志输出到控制台和文件的完整示例。

log4j.xml

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true"
  xmlns:log4j='http://jakarta.apache.org/log4j/'>

	<appender name="console" class="org.apache.log4j.ConsoleAppender">
	    <layout class="org.apache.log4j.PatternLayout">
		<param name="ConversionPattern" 
			value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
	    </layout>
	</appender>

	<appender name="file" class="org.apache.log4j.RollingFileAppender">
	    <param name="append" value="false" />
	    <param name="maxFileSize" value="10MB" />
	    <param name="maxBackupIndex" value="10" />
	    <param name="file" value="${catalina.home}/logs/myStruts1App.log" />
	    <layout class="org.apache.log4j.PatternLayout">
		<param name="ConversionPattern" 
			value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
	    </layout>
	</appender>

	<root>
		<level value="DEBUG" />
		<appender-ref ref="console" />
		<appender-ref ref="file" />
	</root>

</log4j:configuration> 
```

## 参考

1.  [Log4j 1.2 手册](http://web.archive.org/web/20221012180937/https://logging.apache.org/log4j/1.2/manual.html)
2.  [Log4j pattern layout JavaDoc](http://web.archive.org/web/20221012180937/https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/PatternLayout.html)
3.  [log4j.properties 示例](http://web.archive.org/web/20221012180937/http://www.mkyong.com/logging/log4j-log4j-properties-examples/)

<input type="hidden" id="mkyong-current-postId" value="13385">