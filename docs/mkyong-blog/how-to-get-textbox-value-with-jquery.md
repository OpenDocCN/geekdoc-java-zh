# 如何用 jQuery 获取文本框值

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-get-textbox-value-with-jquery/>

要获得文本框的值，可以使用 jQuery **val()** 函数。

举个例子，

1.  $('input:textbox ')。val()–获取文本框值。
2.  $('input:textbox ')。val("新文本消息")–设置文本框值。

## 文本框示例

```java
 <html>
<head>
<title>jQuery get textbox value example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>

<body>

<h1>jQuery get textbox value example</h1>

<h2>TextBox value : <label id="msg"></label></h2>

<div style="padding:16px;">
	TextBox : <input type="textbox" value="Type something"></input>
</div>

<button id="Get">Get TextBox Value</button> 
<button id="Set">Set To "ABC"</button> 
<button id="Reset">Reset It</button>

<script type="text/javascript">
    $("button:#Get").click(function () {

	$('#msg').html($('input:textbox').val());

    });

    $("button:#Reset").click(function () {

	$('#msg').html("");
	$('input:textbox').val("");

    });

    $("button:#Set").click(function () {

	$('input:textbox').val("ABC");
	$('#msg').html($('input:textbox').val());

    });

</script>

</body>
</html> 
```

[http://web.archive.org/web/20220618072948if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-textbox-value.html](http://web.archive.org/web/20220618072948if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-textbox-value.html)

[Try Demo](http://web.archive.org/web/20220618072948/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-get-textbox-value.html)<input type="hidden" id="mkyong-current-postId" value="5028">