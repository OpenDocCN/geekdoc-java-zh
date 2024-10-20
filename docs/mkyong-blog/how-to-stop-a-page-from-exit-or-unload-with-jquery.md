# 如何用 jQuery 阻止页面退出或卸载

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-stop-a-page-from-exit-or-unload-with-jquery/>

当页面退出或卸载时，' **unload** '事件将被激活，但是该事件不能阻止页面退出。

```java
 $(window).bind('unload', function(){}); 
```

从 jQuery 1.4 开始，您可以将“ **beforeunload** ”事件绑定到$(windows)对象，以阻止页面退出、卸载或导航。

```java
 $(window).bind('beforeunload', function(){ 
	return '';
}); 
```

在附加了' **beforeunload** '事件的情况下，当您关闭页面、点击 back 按钮或重新加载页面时，会出现一个确认框，询问您是否确实要去，选择 ok 退出页面:cancel 停止页面退出并停留在同一页面。

此外，您还可以在确认框中添加自己的信息:

```java
 $(window).bind('beforeunload', function(){
	return '>>>>>Before You Go<<<<<<<< \n Your custom message go here';
}); 
```

*另外，从 jQuery 1.4 开始支持“卸载前”事件*

## 你自己试试

```java
 <html>
<head>
<title>Refresh a page in jQuery</title>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

</head>

<body>

<h1>Stop a page from exit with jQuery</h1>

<button id="reload">Refresh a Page in jQuery</button>

<script type="text/javascript">

	$('#reload').click(function() {

	 	location.reload();

	});

	$(window).bind('beforeunload', function(){
		return '>>>>>Before You Go<<<<<<<< \n Your custom message go here';
	});

</script>

</body>
</html> 
```

[Try Demo](http://web.archive.org/web/20190227163517/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-stop-page-from-exit.html)[jquery](http://web.archive.org/web/20190227163517/http://www.mkyong.com/tag/jquery/)![](img/db8a084558f39b438f5d3fa938cc2885.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190227163517/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5190">







