# jQuery slideUp()、slideDown()和 slideToggle()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-slideup-slidedown-and-slidetoggle-example/>

jQuery 提供了三种简便的方法来轻松创建滑动效果。

1.  slide up()–隐藏具有向上滑动效果的匹配元素。
2.  slide down()–以向下滑动的效果显示匹配的元素。
3.  slide toggle()–如果匹配的元素被显示，它将以向上滑动的效果隐藏；如果隐藏，它将以向下滑动的效果显示。

以上三种方法都支持持续时间作为参数:慢速、快速或精确的毫秒。如果省略此参数，则应用默认的 400 毫秒。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	.slideDownbox, .slideUpbox, .slideTogglebox{
		float:left;
		padding:8px;
		margin:16px;
		border:1px solid red;
		width:200px;
		height:50px;
		background-color:#000000;
		color:white;
	}
	.clear{
		clear:both;
	}
</style>

</head>

<body>

<h1>jQuery slideUp(), slideDown() and slideToggle() example</h1>

<div class="clear">
	<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>slideDown() example</h2>

	<div class="slideDownbox">
		Click me - slideDown()
	</div>

	<div class="slideDownbox">
		Click me - slideDown()
	</div>
</div>

<div class="clear">
	<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>slideUp() example</h2>

	<div class="slideUpbox">
		Click me - slideUp()
	</div>

	<div class="slideUpbox">
		Click me - slideUp()
	</div>
</div>

<div class="clear">
	<h2>slideToggle() example</h2> <button id=slideToggle>slideToggle()</button>
	<br/>
	<div class="slideTogglebox">
	 	slideToggle()
	</div>

	<div class="slideTogglebox">
	 	slideToggle()
	</div>

</div>

<br/><br/>

<div class="clear">
	<button id=reset>Reset</button>
</div>

<script type="text/javascript">

$(".slideDownbox").click(function () {
   $(this).hide().slideDown('slow');
});

$(".slideUpbox").click(function () {
   $(this).slideUp(2000);
});

$("#slideToggle").click(function () {
   $('.slideTogglebox').slideToggle();
});

$("#reset").click(function(){
	location.reload();
});
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190304003356if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-slideUp-slideDown-slideToggle-example.html](http://web.archive.org/web/20190304003356if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-slideUp-slideDown-slideToggle-example.html)

[Try Demo](http://web.archive.org/web/20190304003356/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-slideUp-slideDown-slideToggle-example.html)[jquery](http://web.archive.org/web/20190304003356/http://www.mkyong.com/tag/jquery/) [jquery effects](http://web.archive.org/web/20190304003356/http://www.mkyong.com/tag/jquery-effects/)![](img/4642ba5576a7fc5a1eda16d9309fa209.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304003356/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5266">







