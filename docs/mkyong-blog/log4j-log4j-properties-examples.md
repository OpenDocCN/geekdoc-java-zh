# log4j.properties 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/logging/log4j-log4j-properties-examples/>

我找不到太多的`log4j.properties`例子，这里有几个`log4j.properties`例子是我的项目中用到的，仅供分享。

## 1.输出到控制台

所有日志将被重定向到您的控制台。

log4j.properties

```java
 # Root logger option
log4j.rootLogger=INFO, stdout

# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n 
```

## 3.输出到文件

所有记录将被重定向到您指定的日志文件。

log4j.properties

```java
 # Root logger option
log4j.rootLogger=INFO, file

# Direct log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender

#Redirect to Tomcat logs folder
#log4j.appender.file.File=${catalina.home}/logs/logging.log

log4j.appender.file.File=C:\\logigng.log
log4j.appender.file.MaxFileSize=10MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n 
```

## 3.输出到控制台和文件

所有日志记录将被重定向到日志文件和控制台。

log4j.properties

```java
 # Root logger option
log4j.rootLogger=INFO, file, stdout

# Direct log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=C:\\logging.log
log4j.appender.file.MaxFileSize=10MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n 
```

**Note**
If you prefer XML, please refer to this [log4j.xml example](http://web.archive.org/web/20220618160457/http://www.mkyong.com/logging/log4j-xml-example).

## 参考

1.  [Log4j 手动](http://web.archive.org/web/20220618160457/https://logging.apache.org/log4j/1.2/manual.html)
2.  [Log4j 图形布局手册](http://web.archive.org/web/20220618160457/https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/PatternLayout.html)

<input type="hidden" id="mkyong-current-postId" value="2670">