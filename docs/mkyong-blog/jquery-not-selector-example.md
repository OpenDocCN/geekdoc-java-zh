# jQuery–非选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-not-selector-example/>

在 jQuery 中，使用“**而不是**来选择所有与选择器不匹配的元素。

例子

1.  $('p:不是(。class-p1)')–选择由

    匹配的、没有“class-P1”类名的所有元素。

2.  $(' Li:NOT(:only-child)')–选择与
3.  匹配的所有元素，这些元素不是其父元素的唯一子元素。
4.  $(' Li:NOT(:first-child)')–选择所有与匹配但不是其父元素的第一个子元素的元素。

## jQuery 示例

一个简单的例子展示了 jQuery 的“not”选择器的用法，点击按钮来玩转它。

```java
 <html>
<head>
<title>jQuery not example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
</head>

<body>

<h1>jQuery not example</h1>

<ul class="class-ul">
	<li>class-ul - #1</li>
	<li>class-ul - #2</li>
	<li>class-ul - #3</li>
	<li>class-ul - #4</li>
	<li>class-ul - #5</li>
</ul>

<ul id="id1">
	<li>id1 - #1</li>
</ul>

<p class="class-p1">
	class - #p1
</p>

<p class="class-p2">
	class - #p2
</p>

<button>p:not(.class-p1)</button>
<button>li:not(:only-child)</button>
<button>li:not(:first-child)</button>

<script type="text/javascript">
    $("button").click(function () {
      var str = $(this).text();
      $("li,p").css("background", "white");
      $(str).css("background", "coral");
    });
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190224163755if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-not-example.html](http://web.archive.org/web/20190224163755if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-not-example.html)

[Try Demo](http://web.archive.org/web/20190224163755/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-not-example.html)[jquery](http://web.archive.org/web/20190224163755/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190224163755/http://www.mkyong.com/tag/jquery-selector/)![](img/9db6078f5aa31a7db2304bd52db3c5f7.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224163755/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="4992">







