# Wicket 文件上传验证器没有执行？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/wicket/wicket-fileupload-validator-is-not-execute/>

## 问题

为 FileUpload 组件实现了一个自定义验证器，参见代码片段…

```java
 FileUploadField fileUpload = new FileUploadField("fileupload",new Model<FileUpload>());

fileUpload .add(new AbstractValidator() { 

	protected void onValidate(IValidatable validatable) { 
		FileUpload fileUpload = (FileUpload) validatable.getValue();	
		//validate fileUpload
	}

    protected String resourceKey() {
	    return "yourErrorKey";
	}

}); 
```

然而，如果用户没有选择任何文件上传，并点击提交按钮，附加的上传验证程序将被忽略！？

 ## 解决办法

默认情况下， **AbstractValidator** (您的自定义验证器)**不会对空值**进行验证，参见源代码:

*文件:抽象分隔符. java*

```java
 * @see IValidator#validate(IValidatable)
	 */
	public final void validate(IValidatable<T> validatable)
	{
		if (validatable.getValue() != null || validateOnNullValue())
		{
			onValidate(validatable);
		}
	} 
```

要修复它，只需像这样覆盖`validateOnNullValue()`方法:

```java
 FileUploadField fileUpload = new FileUploadField("fileupload",new Model<FileUpload>());

fileUpload .add(new AbstractValidator() { 

       public boolean validateOnNullValue(){
	        return true;
	}

	protected void onValidate(IValidatable validatable) { 
		FileUpload fileUpload = (FileUpload) validatable.getValue();	
	}

    protected String resourceKey() {
	    return "yourErrorKey";
	}

}); 
```

现在，当没有选择文件，并点击提交按钮，验证将被执行。

[file upload](http://web.archive.org/web/20190120162100/http://www.mkyong.com/tag/file-upload/) [validation](http://web.archive.org/web/20190120162100/http://www.mkyong.com/tag/validation/) [wicket](http://web.archive.org/web/20190120162100/http://www.mkyong.com/tag/wicket/)![](img/ff806ceb158e92368344950bf7bb3369.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190120162100/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1676">







