# 下载/导出 Google 应用程序引擎日志、Java 应用程序

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/download-export-google-app-engine-logs-java-app/>

在 GAE，Java App，你可以使用命令" **appcfg request_logs** "从 GAE 下载或导出日志到你的电脑，见下面签名:

```java
 AppCfg [options] request_logs <app-dir> <output-file> 
```

您可以输入“ **appcfg request_logs** ”来查看所有可用选项。

## 1.什么是`<app-dir>`？

这不是您的 app_id，也不是来自 GAE 的任何已部署的 web 路径，这是您的本地项目文件夹，包含文件:

1.  WEB-INF/appengine-web.xml
2.  WEB-INF/web.xml

GAE Java SDK，命令“ **appcfg** ”是硬编码的，可以找到上面两个文件。要从 GAE 下载日志，您必须在“ **WEB-INF** ”文件夹中提供上述两个 XML 文件，这是没有意义的，但您别无选择，只能遵循 GAE 的做法。

在我看来，更好的方法应该是

```java
 AppCfg [options] request_logs <app-id-on-GAE> <output-file> 
```

**Note**
If you don’t have the XML files but still want to download the logs, you can issue [appcfg download_app](http://web.archive.org/web/20190227044303/http://www.mkyong.com/google-app-engine/download-uploaded-application-from-google-app-engine-gae/) to download the deployed application which included the XML files. ## 2.例子

以下是一些从 GAE 下载日志的常见例子。

1.从 GAE 下载今天的日志到“c:\gae.log”。

```java
 C:\appengine-java-sdk-1.6.3.1\bin> 
appcfg request_logs C:\workspace-3.7\SpringGAEProject\war\ c:\gae.log 
```

2.将 GAE 的所有日志下载到“c:\gae.log”。

```java
 C:\appengine-java-sdk-1.6.3.1\bin>
appcfg --num_days=0 request_logs C:\workspace-3.7\SpringGAEProject\war\ c:\gae.log 
```

默认情况下，–天数=1

3.将严重性=4 的所有日志从 GAE 下载到“c:\gae.log”。

```java
 C:\appengine-java-sdk-1.6.3.1\bin>
appcfg --num_days=0 --severity=4 request_logs C:\workspace-3.7\SpringGAEProject\war\ c:\gae.log 
```

*P.S Severity 是日志中的记录级别。范围是 0(调试)到 4(关键)。*

4.从 GAE 下载所有日志并附加到“c:\gae.log”。

```java
 C:\appengine-java-sdk-1.6.3.1\bin>
appcfg --num_days=0 --append request_logs C:\workspace-3.7\SpringGAEProject\war\ c:\gae.log 
```

 ## 参考

1.  [GAE 下载日志文档](http://web.archive.org/web/20190227044303/https://developers.google.com/appengine/docs/java/tools/uploadinganapp#Downloading_Logs)
2.  [下载应用引擎日志](http://web.archive.org/web/20190227044303/http://blog.dantup.com/2009/12/downloadingexporting-app-engine-logs.html)

[export](http://web.archive.org/web/20190227044303/http://www.mkyong.com/tag/export/) [gae](http://web.archive.org/web/20190227044303/http://www.mkyong.com/tag/gae/) [logs](http://web.archive.org/web/20190227044303/http://www.mkyong.com/tag/logs/)







