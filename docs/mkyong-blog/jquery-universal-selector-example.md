# jQuery–通用*选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-universal-selector-example/>

**通用*** 选择器用于选择所有元素，一切。

例子

1.  $('* '):选择文档中的所有元素。
2.  $('div > *):选择作为元素的子元素的所有元素。

总的来说，单独使用通用没有意义，至少我想不出任何用例。但是，它总是被用来与其他元素组合形成一个新的自定义选择器表达式。

## jQuery 示例

一个简单的例子展示了 jQuery universal *选择器的用法。

```java
 <html>
<head>
<title>jQuery universal example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div{
		padding:8px;
	}
</style>
</head>

<body>

<h1>jQuery universal example</h1>

<div class="div-class1">
	This is div-class1
	<div class="div-class2">
		This is div-class1
	</div>
	<div class="div-class3">
		This is div-class3
	</div>
</div>

<br/><br/>
<button>*</button>
<button>div > *</button>
<button id="refresh">Refresh</button>

<script type="text/javascript">
    $("button").click(function () {
       var str = $(this).text();
       $(str).css("border", "1px solid #ff0000");
    });

    $('#refresh').click(function() {
    	location.reload();
    });

</script>

</body>
</html> 
```

[http://web.archive.org/web/20190224162548if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-universal-example.html](http://web.archive.org/web/20190224162548if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-universal-example.html)

[Try Demo](http://web.archive.org/web/20190224162548/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-universal-example.html)[jquery](http://web.archive.org/web/20190224162548/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190224162548/http://www.mkyong.com/tag/jquery-selector/)![](img/072ef730b210efe94e59742cd87c9682.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224162548/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5006">







