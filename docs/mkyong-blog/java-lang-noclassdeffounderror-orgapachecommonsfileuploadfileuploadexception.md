# Java . lang . noclassdeffounderror:org/Apache/commons/file upload/file upload exception

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts/java-lang-noclassdeffounderror-orgapachecommonsfileuploadfileuploadexception/>

## 问题

在 Struts 框架中，在文件上传过程中遇到以下异常。

```java
 javax.servlet.ServletException: Servlet execution threw an exception

root cause

java.lang.NoClassDefFoundError: org/apache/commons/fileupload/FileUploadException
	java.lang.Class.getDeclaredConstructors0(Native Method)
	java.lang.Class.privateGetDeclaredConstructors(Unknown Source)
	java.lang.Class.getConstructor0(Unknown Source)
	java.lang.Class.newInstance0(Unknown Source)
	java.lang.Class.newInstance(Unknown Source) 
```

 ## 解决办法

Struts 使用“ **commons-fileupload.jar** ”库进行文件上传过程。您必须将此库包含到项目依赖项库文件夹中。

1.从 http://commons.apache.org/fileupload/[官方网站](http://web.archive.org/web/20190223081913/http://commons.apache.org/fileupload/)获取“ **commons-fileupload.jar**

2.从 Maven 存储库中获取" **commons-fileupload.jar** "

```java
 <dependency>
      <groupId>commons-fileupload</groupId>
	  <artifactId>commons-fileupload</artifactId>
      <version>1.2.1</version>
    </dependency> 
```

[struts](http://web.archive.org/web/20190223081913/http://www.mkyong.com/tag/struts/)![](img/84da20dc70b20e3deae742815a6ad88f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190223081913/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4547">







