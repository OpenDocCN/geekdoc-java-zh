# jQuery–访问受限 URI 被拒绝–解决方案

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-access-to-restricted-uri-denied-solution/>

## 问题

这个 jQuery 错误消息是由加载跨域内容引起的。

```java
 Error: [Exception... "Access to restricted URI denied"  
code: "1012" nsresult: "0x805303f4 (NS_ERROR_DOM_BAD_URI)" 
```

这意味着你正在加载一些不属于或不在你的网站上的内容(不同的域名)。参见这个 jQuery 例子，按需加载跨域(yahoo.com)内容。

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>
<style type="text/css">
	#content{
		border:1px solid blue;
		margin:16px;
		padding:16px;
	}
</style>

</head>
<body>
  <div id="msg"></div>

  <div id="content">
  </div>

  <br/>
  <button id="load">Load yahoo.com</button>

<script type="text/javascript">

$('#load').click(function(){
	$('#msg').text("Loading......");
	$('#content').load("http://www.yahoo.com", function() {
 		$('#msg').text("");
	});
});

</script>

</body>
</html> 
```

然而，这不起作用，当你点击“加载”按钮时，它什么也不做，只是提示一个“**访问受限 URI 被拒绝**”的错误信息。**由于 JavaScript 安全限制，严格不允许跨域加载内容**。

 ## 解决办法

这里有一个肮脏的解决方法——用服务器端语言获取跨域内容。例如，创建一个名为“proxy.php”的单行 php 文件。

**proxy.php**

```java
 <?php echo file_get_contents($_GET['url']);?> 
```

在 jQuery 端，将 load 函数改为

```java
 $('#load').click(function(){
	$('#msg').text("Loading......");
	$('#content').load("proxy.php?url=http://www.yahoo.com", function() {
 		$('#msg').text("");
	});
});
</script> 
```

[Try Demo](http://web.archive.org/web/20190225225008/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-cross-domain-load-data-example.html)

现在，当你点击“加载”按钮，它会加载跨域(yahoo.com)的内容到你的网页上点播。

[jquery](http://web.archive.org/web/20190225225008/http://www.mkyong.com/tag/jquery/) [uri](http://web.archive.org/web/20190225225008/http://www.mkyong.com/tag/uri/)![](img/4481a3d99f5fa1394434ba7df733dc3b.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225225008/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5296">







