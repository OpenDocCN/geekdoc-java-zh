# jQuery mousemove()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-mousemove-example/>

当鼠标在匹配的元素中移动时，触发 **mousemove()** 事件，当鼠标在匹配的元素周围移动每一个像素时，保持触发该事件。

比如一个 300 x 200 的大 div 框，id 为“bigbigbox”。

```java
 <style type="text/css">
	#bigbigbox{
		margin:16px 16px 16px 48px;
		border:1px groove blue;
		background-color : #BBBBBB;
		width:300px;
		height:200px;
	}
	#msg{
		color:#800000;
	}
</style>

<div id="bigbigbox"></div> 
```

将 **mousemove()** 事件绑定到这个大盒子上。

```java
 $('#bigbigbox').mousemove(function(event) {
  var msg = 'mousemove() position - x : ' + event.pageX + ', y : ' + event.pageY;
}); 
```

当鼠标在 div " **bigbigbox** 元素中移动时， **mousemove()** 事件被触发，您可以使用**事件。PageX** 和 **event.pageY** 跟踪鼠标在框内的位置。

## 用例？

在哪里应用这个 **mousemove()** 事件？知道的请建议一些:)

 ## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#bigbigbox{
		margin:16px 16px 16px 48px;
		border:1px groove blue;
		background-color : #BBBBBB;
		width:300px;
		height:200px;
	}
	#msg{
		color:#800000;
	}
</style>

</head>
<body>
  <h1>jQuery mousemove example</h1>

  <div id="bigbigbox"></div>

  <p id="msg"></p>

<script type="text/javascript">

var i=0;
$('#bigbigbox').mousemove(function(event) {
  var msg = 'mousemove() position - x : ' + event.pageX + ', y : '
                + event.pageY + ' [counter] : ' + i++;
  $('#msg').text(msg);
});

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190210095225if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mousemove-example.html](http://web.archive.org/web/20190210095225if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mousemove-example.html)

[Try Demo](http://web.archive.org/web/20190210095225/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mousemove-example.html)[jquery](http://web.archive.org/web/20190210095225/http://www.mkyong.com/tag/jquery/) [mouse movement](http://web.archive.org/web/20190210095225/http://www.mkyong.com/tag/mouse-movement/)![](img/8a57b84c448622984b007e53eb27c530.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190210095225/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5207">







