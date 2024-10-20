# 使用 jQuery 的 Mashable 浮动效果示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/mashable-floating-effect-example-with-jquery/>

Mashable 最出名的是社交媒体资源网站，当用户滚动页面时，它创造了一个令人敬畏的浮动效果。这里有一个简单的想法，用 jQuery 克隆这种浮动效果。

![](img/dac5584a9e6b1dc872510e77b935dff5.png "Mashable-floating-effect-example")

## 想法…

1.  创建一个浮动框。
2.  初始浮动框的位置，将其放在正文内容的旁边。
3.  当用户滚动页面时，不断检查滚动条的位置。
4.  如果滚动条 y 位置大于浮动框 y 位置，动态改变浮动框 y 位置。
5.  当滚动条 y 位置小于浮动框 y 位置时，恢复原始位置。
6.  当然是用 jQuery。

 ## 1.HTML 布局

一个简单的 HTML 布局，页眉，内容和页脚，在内容上方放置一个 div“浮动框”。

```java
 <div id="page">
  	<div id="header"><h1>header</h1></div>
	<div id="floating-box"></div>
	<div id="body">
		<h1>content</h1>
		<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script><h2>Mashable floating effect example</h2>
	</div>
	<div id="footer"><h1>footer</h1></div>
</div> 
```

## 2.浮箱 90×200

当人们滚动盒子时，这个盒子会平稳地浮动。您可能需要调整**"左边距:-100px；**“有点适合你的需要。

```java
 #floating-box{
	width:90px;
	height:200px;
	border:1px solid red;
	background-color:#BBBBBB;
	float:left;
	margin-left:-100px;
	margin-right:10px;
	position:absolute;
	z-index:1;
} 
```

## 3.请不要有冲突

确保 jQuery 与其他库没有冲突。强烈推荐去查一下。

```java
 //avoid conflict with other script
    $j=jQuery.noConflict();

    $j(document).ready(function($) { 
```

## 4.位置，位置，位置

绑定 jQuery scroll()事件以持续检查浏览器的滚动条位置。

```java
 $(window).scroll(function () { 

	var scrollY = $(window).scrollTop();
	var isfixed = $floatingbox.css('position') == 'fixed';

	if($floatingbox.length > 0){

		if ( scrollY > bodyY && !isfixed ) {
			$floatingbox.stop().css({
				position: 'fixed',
				left: '50%',
				top: 20,
				marginLeft: -500
			});
		} else if ( scrollY < bodyY && isfixed ) {
			$floatingbox.css({
				position: 'relative',
				left: 0,
				top: 0,
				marginLeft: originalX
			});
		}		
	}	
}); 
```

如果滚动条 y 位置大于浮动框 y 位置，将浮动框 y 位置改为“ **marginLeft: -500** ”。您可能需要自定义该值以满足您的需要。

```java
 if ( scrollY > bodyY && !isfixed ) {
	   $floatingbox.stop().css({
		position: 'fixed',
		left: '50%',
		top: 20,
		marginLeft: -500
	   });
	 } 
```

如果滚动条的 y 位置小于浮动框的 y 位置，恢复到原始位置。

```java
 if ( scrollY < bodyY && isfixed ) {
	   $floatingbox.css({
		position: 'relative',
		left: 0,
		top: 0,
		marginLeft: originalX
	   });
	} 
```

## 5.完成的

试着播放下面的例子来了解作品。

这个浮动效果功能是在我的[Digg WordPress 插件](http://web.archive.org/web/20190310101552/http://www.mkyong.com/blog/digg-digg-wordpress-plugin/)中实现的。

## 你自己试试

```java
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html>
<head>
<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#floating-box{
		width:90px;
		height:200px;
		border:1px solid red;
		background-color:#BBBBBB;
		float:left;
		margin-left:-100px;
		margin-right:10px;
		position:absolute;
		z-index:1;
	}
	#page{
		width:800px;
    	margin:0 auto;
	}
	#header{
		border:1px solid blue;
		height:100px;
		margin:8px;
	}
	#body{
		border:1px solid blue;
		height:2400px;
		margin:8px;
	}
	#footer{
		border:1px solid blue;
		height:100px;
		margin:8px;
	}
	h1,h2{
		padding:16px;
	}
</style>

</head>
<body>

  <div id="page">
  	<div id="header"><h1>header</h1></div>

	<div id="floating-box">

	</div>

	<div id="body">
		<h1>content</h1>
		<h2>Mashable floating effect example</h2>
	</div>
	<div id="footer"><h1>footer</h1></div>
  </div>

<script type="text/javascript">

    //avoid conflict with other script
    $j=jQuery.noConflict();

    $j(document).ready(function($) {

	//this is the floating content
	var $floatingbox = $('#floating-box');

	if($('#body').length > 0){

	  var bodyY = parseInt($('#body').offset().top) - 20;
	  var originalX = $floatingbox.css('margin-left');

	  $(window).scroll(function () { 

	   var scrollY = $(window).scrollTop();
	   var isfixed = $floatingbox.css('position') == 'fixed';

	   if($floatingbox.length > 0){

	      $floatingbox.html("srollY : " + scrollY + ", bodyY : " 
                                    + bodyY + ", isfixed : " + isfixed);

	      if ( scrollY > bodyY && !isfixed ) {
			$floatingbox.stop().css({
			  position: 'fixed',
			  left: '50%',
			  top: 20,
			  marginLeft: -500
			});
		} else if ( scrollY < bodyY && isfixed ) {
		 	  $floatingbox.css({
			  position: 'relative',
			  left: 0,
			  top: 0,
			  marginLeft: originalX
		});
	     }		
	   }
       });
     }
  });
</script>
</body>
</html> 
```

[Try Demo](http://web.archive.org/web/20190310101552/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-Mashable-floating-effect-example.html)[jquery](http://web.archive.org/web/20190310101552/http://www.mkyong.com/tag/jquery/) [jquery effects](http://web.archive.org/web/20190310101552/http://www.mkyong.com/tag/jquery-effects/)![](img/db07b12801c98faa40e92b81f1f7870d.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190310101552/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5255">







