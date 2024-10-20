# jQuery find()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-find-example/>

使用 jQuery，您可以使用`find()`来搜索匹配元素的所有后代(子元素、孙元素、曾孙元素……任何深度级别)。

例如，`div`元素有三层深度。

```java
 <div id="A1">

	<div class="child">A1-1</div>
	<div class="child">A1-2</div>

	<div id="A2">

		<div class="child">A2-1</div>
		<div class="child">A2-2</div>

		<div id="A3">

			<div class="child">A3-1</div>
			<div class="child">A3-2</div>

		</div>

	</div>

</div> 
```

## 1.$('#A1 ')。查找('。孩子’)

```java
 $('#A1').find('.child').css('background','red'); 
```

找到包含 id 为“`A1`”的元素，以及包含类名“**子**”的子元素，然后将其背景更改为红色。

 ## 2.$('#A2 ')。查找('。孩子’)

```java
 $('#A2').find('.child').css('background','red'); 
```

找到包含 id 为“`A2`”的元素，以及包含类名“**子**”的子元素，然后将其背景更改为红色。

 ## 3.jQuery find()示例

播放这个例子。

HTML example

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div{
		padding:8px;
		border:1px solid;
	}
</style>

</head>

<body>

<h1>jQuery find() example</h1>

<script type="text/javascript">

  $(document).ready(function(){

    $("#button1").click(function () {

	$('div').css('background','white');

	$('#A1').find('.child').css('background','red');

    });

    $("#button2").click(function () {

	$('div').css('background','white');

	$('#A2').find('.child').css('background','red');

    });

    $("#button3").click(function () {

	$('div').css('background','white');

	$('#A3').find('.child').css('background','red');

    });

  });
</script>
</head>
<body>

<div id="A1">A1
	<div class="child">A1-1</div>
	<div class="child">A1-2</div>

	<div id="A2">A2
		<div class="child">A2-1</div>
		<div class="child">A2-2</div>

		<div id="A3">A3
			<div class="child">A3-1</div>
			<div class="child">A3-2</div>
		</div>
	</div>

</div>

<br/>
<br/>
<br/>

<input type='button' value='find #A1' id='button1'>
<input type='button' value='find #A2' id='button2'>
<input type='button' value='find #A3' id='button3'>

</body>
</html> 
```

[http://web.archive.org/web/20190224153456if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-example.html](http://web.archive.org/web/20190224153456if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-example.html)

## 参考

1.  [jQuery find() API 文档](http://web.archive.org/web/20190224153456/http://api.jquery.com/find/)

[jquery](http://web.archive.org/web/20190224153456/http://www.mkyong.com/tag/jquery/) [jquery traversing](http://web.archive.org/web/20190224153456/http://www.mkyong.com/tag/jquery-traversing/)







