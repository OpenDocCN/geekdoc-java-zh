# Java . lang . noclassdeffounderror:org/Apache/commons/io/output/DeferredFileOutputStream

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/java-lang-noclassdeffounderror-orgapachecommonsiooutputdeferredfileoutputstream/>

## 问题

在 Struts 框架中执行一些 I/O 操作，但是在文件上传过程中遇到以下异常。

```java
 java.lang.NoClassDefFoundError: 
        org/apache/commons/io/output/DeferredFileOutputStream 
```

从哪里下载 Apache commons-io？

 ## 解决办法

默认情况下，Struts 使用 Apache " **commons-io.jar** "进行文件上传。要修复它，您必须将此库包含到项目依赖库文件夹中。

 ## 1.直接得到

从 http://commons.apache.org/io/[官方网站](http://web.archive.org/web/20190209023054/http://commons.apache.org/io/)获取“ **commons-io.jar**

## 2.从 Maven 获取

更好的方法是从 Maven 存储库中获取" **commons-io.jar** "

*文件:pom.xml*

```java
 <dependency>
      <groupId>commons-io</groupId>
	  <artifactId>commons-io</artifactId>
      <version>1.4</version>
    </dependency> 
```

[struts](http://web.archive.org/web/20190209023054/http://www.mkyong.com/tag/struts/)







