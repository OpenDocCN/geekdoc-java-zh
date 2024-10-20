# Spring MVC 无法转换文件上载表单中的属性值

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/spring-mvc/spring-mvc-failed-to-convert-property-value-in-file-upload-form/>

## 问题

在 Spring MVC 应用中，在点击文件上传按钮的同时，命中以下属性类型转换错误？

 *<font color="red">无法将[org . spring framework . web . multipart . commons . commonsmultipartfile]类型的属性值转换为属性文件所需的类型[byte[]];嵌套异常为 Java . lang . illegalargumentexception:无法将[org . spring framework . web . multipart . commons . commonsmultipartfile]类型的值转换为属性文件[0]所需的类型[byte]:property editor[org . spring framework . beans . property editors . customnumbereditor]返回了不适当的值</font>*

下面是 SimpleFormController …

```java
 public class FileUploadController extends SimpleFormController{

	public FileUploadController(){
		setCommandClass(FileUpload.class);
		setCommandName("fileUploadForm");
	}
	//...

public class FileUpload{

	byte[] file;
	//...
} 
```

 ## 解决办法

这是在 Spring MVC 中处理上传文件的常见问题，它无法自动将上传文件转换为字节数组。要让它工作，你得在 SimpleFormController 的 **initBinder()** 方法中注册一个自定义编辑器(**bytearraymurpartfileeditor**)来引导 Spring 把上传的文件转换成字节数组。

```java
 public class FileUploadController extends SimpleFormController{

	public FileUploadController(){
		setCommandClass(FileUpload.class);
		setCommandName("fileUploadForm");
	}

       @Override
	protected void initBinder(HttpServletRequest request, ServletRequestDataBinder binder)
		throws ServletException {

		// Convert multipart object to byte[]
		binder.registerCustomEditor(byte[].class, new ByteArrayMultipartFileEditor());

	}
	//... 
```

[file upload](http://web.archive.org/web/20190220130945/http://www.mkyong.com/tag/file-upload/) [spring mvc](http://web.archive.org/web/20190220130945/http://www.mkyong.com/tag/spring-mvc/)![](img/0e4caee0d3a1df77015a96eff421d12a.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190220130945/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6778">







