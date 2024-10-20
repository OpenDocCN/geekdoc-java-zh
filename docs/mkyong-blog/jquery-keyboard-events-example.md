# jQuery 键盘事件示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-keyboard-events-example/>

jQuery 附带了三个键盘事件来捕获键盘活动——**keyup()**、 **keydown()** 和 **keypress()** 。

1.  keyup()-当用户释放键盘上的一个键时触发。
2.  keydown()-当用户按下键盘上的一个键时触发。
3.  keypress()–当用户按下键盘上的一个键时激发。

一般来说， **keydown()** 类似于 **keypress()** 事件。实际上，**按键()**和**按键()**事件之间几乎没有区别。

## 1.重复键

如果您按住某个键，keydown()事件会被触发一次，但是 keypress()事件会一直触发，直到您放开该键。

 ## 2.编辑关键点

键盘修饰键( **ctrl、shift、alt…** )将触发 keydown()事件，但不会触发 keypress()事件。

 ## 3.key code-ascii 代码

比如 A = 65，a= 97，请参考这个 [ASCII 表图表](http://web.archive.org/web/20190303105502/http://www.asciitable.com/)。

1.  keydown()和 keyup()将显示 a = 65，A = 65(不区分大小写-小写和大写将显示相同的值)。
2.  keypresss()将显示 a= 97，A=65(区分大小写-小写和大写将显示不同的值)。

如果您想在中捕获真正的字符键，请使用 keypress()事件。

## FireFox 中不显示键码？

**event.keyCode** 在 FireFox 中不工作，但在 IE 中工作正常。要在 Firefox 中获得 **event.keyCode** ，你应该使用 **event.which** 来代替，jQuery 也推荐它。所以更好的方法应该是

```java
 var keycode = (event.keyCode ? event.keyCode : event.which); 
```

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	span{
		padding:8px;
		margin:8px;
		color:blue;
	}
	div{
		padding:8px;
		margin:8px;
	}
</style>

</head>
<body>
  <h1>jQuery keyup(), keydown() and keypress() example</h1>

<label>TextBox : </label>
<input id="textbox" type="text" size="50" />

<div>
<label>1\. keyup() Message :</label> <span id="msg-keyup"></span>
</div>

<div>
<label>2\. keydown() Message :</label><span id="msg-keydown"></span>
</div>

<div>
<label>3\. keypress() Message :</label><span id="msg-keypress"></span>
</div>

<script type="text/javascript">

$('#textbox').keyup(function(event){
	$('#msg-keyup').html('keyup() is triggered!, keyCode = ' 
              + event.keyCode + ' which = ' + event.which)
});

$('#textbox').keydown(function(event){
	$('#msg-keydown').html('keydown() is triggered!, keyCode = ' 
              + event.keyCode + ' which = ' + event.which)
});

$('#textbox').keypress(function(event){
	$('#msg-keypress').html('keypress() is triggered!, keyCode = ' 
              + event.keyCode + ' which = ' + event.which)
});

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190303105502if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-keyboard-events-example.html](http://web.archive.org/web/20190303105502if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-keyboard-events-example.html)

[Try Demo](http://web.archive.org/web/20190303105502/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-keyboard-events-example.html)[jquery](http://web.archive.org/web/20190303105502/http://www.mkyong.com/tag/jquery/) [keyboard event](http://web.archive.org/web/20190303105502/http://www.mkyong.com/tag/keyboard-event/)







