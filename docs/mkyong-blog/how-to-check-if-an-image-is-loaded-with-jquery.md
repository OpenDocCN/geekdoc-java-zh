# 如何检查图片是否加载了 jQuery

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-check-if-an-image-is-loaded-with-jquery/>

要检查图片加载是否成功，可以结合使用 jQuery ' **load()** 和' **error()** 事件:

```java
 $('#image1')
	.load(function(){
		$('#result1').text('Image is loaded!');	
	})
	.error(function(){
		$('#result1').text('Image is not loaded!');
	}); 
```

如果图像加载成功，则调用 **load()** 函数，否则调用 **error()** 函数。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

 <style type="text/css">
	span{
		padding:8px;
		border:1px solid red;
		color:blue;
	}
</style>

</head>

<body>

<h1>Check if image is loaded jQuery</h1>
<p>
<img id="image1" 
src="http://static.jquery.com/files/rockimg/logo_jquery_215x53.gif"/>
<p>Load image from 
"http://static.jquery.com/files/rockimg/logo_jquery_215x53.gif"</p>

<span id="result1"></span>
</p>

<p>
<img id="image2" src="xxxx.jpg"/>
<p>Load image from "xxxx.jpg"</p>

<span id="result2"></span>
</p>

<script type="text/javascript">

$('#image1')
	.load(function(){
		$('#result1').text('Image is loaded!');	
	})
	.error(function(){
		$('#result1').text('Image is not loaded!');
	});

$('#image2')
	.load(function(){
		$('#result2').text('Image is loaded!');	
	})
	.error(function(){
		$('#result2').text('Image is not loaded!');
	});
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190303234728if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-if-image-loaded.html](http://web.archive.org/web/20190303234728if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-if-image-loaded.html)

[Try Demo](http://web.archive.org/web/20190303234728/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-if-image-loaded.html)[image](http://web.archive.org/web/20190303234728/http://www.mkyong.com/tag/image/) [jquery](http://web.archive.org/web/20190303234728/http://www.mkyong.com/tag/jquery/)![](img/d1841c9d1b5031dfbca92dea4653147e.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190303234728/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5194">







