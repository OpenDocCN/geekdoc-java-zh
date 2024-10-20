# jQuery–空选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-empty-selector-example/>

在 jQuery 中，“ **empty** ”选择器用于选择所有没有子元素的元素(包括其中的任何文本)。

例子

```java
 <div class="div-class1">
   This is div-class1
</div>

<div class="div-class2" /> 
```

$(':empty ')–匹配“ **div-class2** ”，不匹配“ **div-class1** ”。

## jQuery 示例

一个简单的例子展示了 jQuery“empty”选择器的用法。

```java
 <html>
<head>
<title>jQuery empty example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div{
		padding:8px;
		border:1px solid;
	}
</style>
</head>

<body>

<h1>jQuery empty example</h1>

<div class="div-class1">
	This is div-class1
</div>

<div class="div-class2" />

<div class="div-class3">
	This is div-class3
	<div class="div-class3-1">
		This is div-class3-1
	</div>	
</div>

<br/><br/>

<button>:empty</button>

<script type="text/javascript">
    $("button").click(function () {
      var str = $(this).text();
      $("*").css("background", "white");
      $(str).css("background", "coral");
    });
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190304031949if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-empty-example.html](http://web.archive.org/web/20190304031949if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-empty-example.html)

[Try Demo](http://web.archive.org/web/20190304031949/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-empty-example.html)[jquery](http://web.archive.org/web/20190304031949/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190304031949/http://www.mkyong.com/tag/jquery-selector/)![](img/5533a0281d60c8df2fec785febe5512d.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304031949/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5001">







