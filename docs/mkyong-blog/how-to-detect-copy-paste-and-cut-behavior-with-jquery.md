# 如何用 jQuery 检测复制、粘贴和剪切行为

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-detect-copy-paste-and-cut-behavior-with-jquery/>

要检测复制、粘贴和剪切行为，只需要绑定相应的事件类型。

```java
 $("#textA").bind('copy', function() {
    $('span').text('copy behaviour detected!')
});	
$("#textA").bind('paste', function() {
    $('span').text('paste behaviour detected!')
});	
$("#textA").bind('cut', function() {
    $('span').text('cut behaviour detected!')
}); 
```

如果您使用的是 jQuery 1.4x，它支持如下的多事件声明:

```java
 $("#textA").bind({
	copy : function(){
		$('span').text('copy behaviour detected!');
	},
	paste : function(){
		$('span').text('paste behaviour detected!');
	},
	cut : function(){
		$('span').text('cut behaviour detected!');
	}
}); 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	span{
		color:blue;
	}
</style>

</head>
<body>
  <h1>jQuery copy, paste and cut example</h1>
  <form action="#">
  	<label>TextBox : </label>
	<input id="textA" type="text" size="50" 
          value="Copy, paste or cut message here" />
  </form>

  <span></span>

<script type="text/javascript">

$(document).ready(function() {

	$("#textA").bind({
		copy : function(){
			$('span').text('copy behaviour detected!');
		},
		paste : function(){
			$('span').text('paste behaviour detected!');
		},
		cut : function(){
			$('span').text('cut behaviour detected!');
		}
	});

});	
</script>
</body>
</html> 
```

[http://web.archive.org/web/20190304001155if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-copy-paste-cut-textbox.html](http://web.archive.org/web/20190304001155if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-copy-paste-cut-textbox.html)

[Try Demo](http://web.archive.org/web/20190304001155/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-copy-paste-cut-textbox.html)[jquery](http://web.archive.org/web/20190304001155/http://www.mkyong.com/tag/jquery/) [keyboard event](http://web.archive.org/web/20190304001155/http://www.mkyong.com/tag/keyboard-event/)![](img/66604c6fcef010e29f79cef7d8412a18.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304001155/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5175">







