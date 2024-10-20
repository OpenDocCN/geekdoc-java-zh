# Struts 2 下载文件示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-download-file-example/>

Download it – [Struts2-Download-File-Example.zip](http://web.archive.org/web/20190304031742/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-Download-File-Example.zip)

一个 Struts 2 的例子，展示了使用自定义结果类型来允许用户下载文件。

## 1.行动

在 Action 类中，声明了 InputStream 数据类型及其 getter 方法。

**DownloadAction.java**

```java
 package com.mkyong.common.action;

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;
import com.opensymphony.xwork2.ActionSupport;

public class DownloadAction extends ActionSupport{

	private InputStream fileInputStream;

	public InputStream getFileInputStream() {
		return fileInputStream;
	}

	public String execute() throws Exception {
	    fileInputStream = new FileInputStream(new File("C:\\downloadfile.txt"));
	    return SUCCESS;
	}
} 
```

 ## 2.查看页面

一个普通的页面，有一个下载文件的链接。

**下载页. jsp**

```java
 <%@ taglib prefix="s" uri="/struts-tags" %>
<html>

<body>
<h1>Struts 2 download file example</h1>

<s:url id="fileDownload" namespace="/" action="download" ></s:url>

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Download file - <s:a href="%{fileDownload}">fileABC.txt</s:a>
</h2>

</body>
</html> 
```

## 3.struts.xml

定义下载文件细节，不言自明。**<param name = " input name ">**值是动作的 InputStream 属性的名称。

Read this [Struts 2 Stream Result documentation](http://web.archive.org/web/20190304031742/http://struts.apache.org/2.x/docs/stream-result.html) for more detail explanation.

**struts.xml**

```java
 <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
"http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>

<constant name="struts.devMode" value="true" />

<package name="default" namespace="/" extends="struts-default">
   <action name="">
	<result name="success">pages/downloadPage.jsp</result>
   </action>

   <action name="download" class="com.mkyong.common.action.DownloadAction">
	<result name="success" type="stream">
	  <param name="contentType">application/octet-stream</param>
	  <param name="inputName">fileInputStream</param>
	  <param name="contentDisposition">attachment;filename="fileABC.txt"</param>
	  <param name="bufferSize">1024</param>
	</result>
   </action>
</package>

</struts> 
```

## 4.运行它

*http://localhost:8080/struts 2 example/*

![Struts2 download file example](img/d1db7c448ebf08cc94f2ca98d20bda10.png "struts2-download-file-example")

## 参考

1.  [http://struts.apache.org/2.x/docs/stream-result.html](http://web.archive.org/web/20190304031742/http://struts.apache.org/2.x/docs/stream-result.html)
2.  [http://www.iana.org/assignments/media-types/](http://web.archive.org/web/20190304031742/http://www.iana.org/assignments/media-types/)
3.  [http://www . mkyong . com/struts/struts-download-file-from-website-example/](http://web.archive.org/web/20190304031742/http://www.mkyong.com/struts/struts-download-file-from-website-example/)
4.  [http://www . mkyong . com/Java/how-to-download-file-from-website-Java-JSP/](http://web.archive.org/web/20190304031742/http://www.mkyong.com/java/how-to-download-file-from-website-java-jsp/)
5.  [http://struts . Apache . org/2 . x/docs/how-can-we-return-a-text-string-as-the-response . html](http://web.archive.org/web/20190304031742/http://struts.apache.org/2.x/docs/how-can-we-return-a-text-string-as-the-response.html)

[download file](http://web.archive.org/web/20190304031742/http://www.mkyong.com/tag/download-file/) [struts2](http://web.archive.org/web/20190304031742/http://www.mkyong.com/tag/struts2/)![](img/66d1719ec0959a149c79d0b9f60b9740.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304031742/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5814">







