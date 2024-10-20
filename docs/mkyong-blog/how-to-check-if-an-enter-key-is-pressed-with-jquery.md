# 如何用 jQuery 检查是否按下了回车键

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-check-if-an-enter-key-is-pressed-with-jquery/>

“输入”键由代码“13”表示，检查这个 [ASCII 图表](http://web.archive.org/web/20211204045900/http://www.asciitable.com/)。

要检查文本框内是否按下了“enter”键，只需将 keypress()绑定到文本框。

```java
 $('#textbox').keypress(function(event){

	var keycode = (event.keyCode ? event.keyCode : event.which);
	if(keycode == '13'){
		alert('You pressed a "enter" key in textbox');	
	}

}); 
```

要检查页面上是否按下了回车键，请将按键()绑定到 jQuery $(文档)。

```java
 $(document).keypress(function(event){

	var keycode = (event.keyCode ? event.keyCode : event.which);
	if(keycode == '13'){
		alert('You pressed a "enter" key in somewhere');	
	}

}); 
```

在 Firefox 中，你必须使用 **event.which** 来获取键码；而 IE 同时支持**事件.键码**和**事件.哪个**。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

</head>
<body>
  <h1>Check if "enter" is pressed with jQuery</h1>

<label>TextBox : </label>
<input id="textbox" type="text" size="50" />

<script type="text/javascript">

$('#textbox').keypress(function(event){

	var keycode = (event.keyCode ? event.keyCode : event.which);
	if(keycode == '13'){
		alert('You pressed a "enter" key in textbox');	
	}
	event.stopPropagation();
});

$(document).keypress(function(event){

	var keycode = (event.keyCode ? event.keyCode : event.which);
	if(keycode == '13'){
		alert('You pressed a "enter" key in somewhere');	
	}

});

</script>
</body>
</html> 
```

[http://web.archive.org/web/20211204045900if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-enter-key-is-pressed.html](http://web.archive.org/web/20211204045900if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-enter-key-is-pressed.html)

[Try Demo](http://web.archive.org/web/20211204045900/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-enter-key-is-pressed.html)<input type="hidden" id="mkyong-current-postId" value="5238">