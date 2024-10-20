# jQuery–唯一子选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-only-child-selector-example/>

“**唯一子元素**用于选择所有作为其父元素唯一子元素的元素。

例子

1.  $(':only-child ')–选择作为其父元素的唯一子元素的所有元素。
2.  $(' Li:only-child ')–选择与
3.  匹配的所有元素，这些元素是其父元素的唯一子元素。

## jQuery 示例

在这个例子中，当按钮被点击时，只有`<li>DEF #1</li>`将被匹配并动态改变其背景颜色，因为它是其父的唯一子。

```java
 <html>
<head>
<title>jQuery only child example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
</head>

<body>

<h1>jQuery only child example</h1>

<ul>
	<li>ABC #1</li>
	<li>ABC #2</li>
	<li>ABC #3</li>
	<li>ABC #4</li>
	<li>ABC #5</li>
</ul>

<ul>
	<li>DEF #1</li>
</ul>

<ul>
	<li>GHI #1</li>
	<li>GHI #2</li>
</ul>

<button>li:only-child</button>

<script type="text/javascript">
    $("button").click(function () {
      var str = $(this).text();
      $("li").css("background", "white");
      $(str).css("background", "coral");
    });
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190224163435if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-only-child.html](http://web.archive.org/web/20190224163435if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-only-child.html)

[Try Demo](http://web.archive.org/web/20190224163435/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-only-child.html)[jquery](http://web.archive.org/web/20190224163435/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190224163435/http://www.mkyong.com/tag/jquery-selector/)![](img/c3c2efde9a4a0fa702ed32ed282cda25.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224163435/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4983">







