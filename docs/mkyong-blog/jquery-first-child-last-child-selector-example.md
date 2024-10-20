# jQuery–第一个孩子和最后一个孩子选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-first-child-last-child-selector-example/>

**:first-child** 用于选择作为其父元素的第一个子元素的所有元素，这是:n-child(1)的简写。

例子

1.  $(' Li:first-child ')–选择与
2.  匹配的所有元素，这些元素是其父元素的第一个子元素。
3.  $(tr:first-child ')–选择与匹配的所有元素，这些元素是其父元素的第一个子元素。

**:last-child** 用于选择作为其父元素的最后一个子元素的所有元素。

例子

1.  $(' Li:last-child ')–选择与
2.  匹配的所有元素，这些元素是其父元素的最后一个子元素。
3.  $(tr:last-child ')–选择与匹配的所有元素，这些元素是其父元素的最后一个子元素。

## jQuery 示例

一个简单的例子，演示了如何使用 first child 和 last child 函数来动态添加" li "背景色。

```java
 <html>
<head>
<title>jQuery first child and last child example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
</head>

<body>

<h1>jQuery first child and last child example</h1>

<ul>
	<li>li #1</li>
	<li>li #2</li>
	<li>li #3</li>
	<li>li #4</li>
	<li>li #5</li>
</ul>

<button>li:first-child</button>
<button>li:last-child</button>

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

[http://web.archive.org/web/20190214222610if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-first-child-last-child.html](http://web.archive.org/web/20190214222610if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-first-child-last-child.html)

[Try Demo](http://web.archive.org/web/20190214222610/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-first-child-last-child.html)[jquery](http://web.archive.org/web/20190214222610/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190214222610/http://www.mkyong.com/tag/jquery-selector/)![](img/596a286c99f707ce93468988fd2234f7.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214222610/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4974">







