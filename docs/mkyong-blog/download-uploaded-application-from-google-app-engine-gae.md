# 从谷歌应用引擎(GAE)下载上传的应用程序

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/download-uploaded-application-from-google-app-engine-gae/>

您可以使用命令“ **appcfg download_app** ”从 GAE 下载上传的应用程序，请参见下面的签名:

```java
 AppCfg [options] -A app_id [ -V version ] download_app <out-dir> 
```

## AppCfg 下载示例

假设您已经将 app_id 为" **mkyong-springmvc** "的应用程序上传到 GAE，要将其下载回您的计算机，请使用以下命令:

```java
 D:\appengine-java-sdk-1.6.3.1\bin>appcfg -A mkyong-springmvc download_app c:\testing 
```

参见输出:

```java
 D:\appengine-java-sdk-1.6.3.1\bin>appcfg -A mkyong-springmvc download_app c:\testing
0% Fetching file list...
0% Fetching files...
0% [1/43] WEB-INF/lib/repackaged-appengine-tomcat-juli-6.0.29.jar
2% [2/43] WEB-INF/lib/spring-context-3.1.1.RELEASE.jar
4% [3/43] WEB-INF/classes/org/apache/jsp/pages/list_jsp.java
6% [4/43] WEB-INF/classes/META-INF/jdoconfig.xml
9% [5/43] WEB-INF/classes/org/apache/jsp/pages/list_jsp.class
11% [6/43] WEB-INF/lib/repackaged-appengine-jasper-el-6.0.29.jar
13% [7/43] WEB-INF/lib/spring-context-support-3.1.1.RELEASE.jar
16% [8/43] WEB-INF/logging.properties
18% [9/43] WEB-INF/lib/jdo2-api-2.3-eb.jar
20% [10/43] WEB-INF/lib/spring-webmvc-3.1.1.RELEASE.jar
23% [11/43] WEB-INF/lib/repackaged-appengine-jasper-6.0.29.jar
25% [12/43] WEB-INF/lib/repackaged-appengine-ant-1.7.1.jar
27% [13/43] WEB-INF/appengine-web.xml
30% [14/43] WEB-INF/classes/log4j.properties
32% [15/43] WEB-INF/lib/datanucleus-jpa-1.1.5.jar
34% [16/43] WEB-INF/lib/appengine-jsr107cache-1.6.3.1.jar
37% [17/43] WEB-INF/lib/geronimo-jpa_3.0_spec-1.1.1.jar
39% [18/43] WEB-INF/web.xml
41% [19/43] WEB-INF/lib/commons-logging-1.1.1.jar
44% [20/43] WEB-INF/lib/appengine-api-labs-1.6.3.1.jar
46% [21/43] WEB-INF/lib/datanucleus-appengine-1.0.10.final.jar
48% [22/43] WEB-INF/lib/repackaged-appengine-ant-launcher-1.7.1.jar
51% [23/43] WEB-INF/lib/aopalliance-1.0.jar
53% [24/43] favicon.ico
55% [25/43] WEB-INF/lib/repackaged-appengine-jasper-jdt-6.0.29.jar
58% [26/43] WEB-INF/lib/repackaged-appengine-jakarta-jstl-1.1.2.jar
60% [27/43] WEB-INF/lib/spring-expression-3.1.1.RELEASE.jar
62% [28/43] WEB-INF/lib/spring-web-3.1.1.RELEASE.jar
65% [29/43] WEB-INF/lib/spring-aop-3.1.1.RELEASE.jar
67% [30/43] WEB-INF/lib/spring-core-3.1.1.RELEASE.jar
69% [31/43] index.html
72% [32/43] WEB-INF/mvc-dispatcher-servlet.xml
74% [33/43] WEB-INF/lib/repackaged-appengine-jakarta-standard-1.1.2.jar
76% [34/43] WEB-INF/lib/spring-asm-3.1.1.RELEASE.jar
79% [35/43] WEB-INF/lib/datanucleus-core-1.1.5.jar
81% [36/43] WEB-INF/appengine-generated/app.yaml
83% [37/43] pages/list.jsp
86% [38/43] WEB-INF/lib/geronimo-jta_1.1_spec-1.1.1.jar
88% [39/43] WEB-INF/lib/spring-beans-3.1.1.RELEASE.jar
90% [40/43] WEB-INF/lib/jsr107cache-1.1.jar
93% [41/43] WEB-INF/classes/com/mkyong/controller/MovieController.class
95% [42/43] __static__/index.html
97% [43/43] __static__/favicon.ico

download_app completed successfully. 
```

上面的命令将从 GAE，app_id " **mkyong-springmvc** "下载所有东西(类，库，jsp)到你的计算机文件夹" c:\testing "。

 ## 参考

1.  [GAE 命令行参数](http://web.archive.org/web/20190226092206/https://developers.google.com/appengine/docs/java/tools/uploadinganapp#Command_Line_Arguments)

[gae](http://web.archive.org/web/20190226092206/http://www.mkyong.com/tag/gae/)![](img/89cf25c4c69c2094e4d0178e0a66dc5b.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190226092206/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="10978">







