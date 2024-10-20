# jQuery filter()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-filter-example/>

jQuery filter 函数是一个有用的特性，通过使用匹配的选择器或函数的测试，可以从一组匹配的元素中提取元素。

## 1.过滤器(选择器)

在一组匹配的元素中，只获取与 filter()选择器匹配的元素。

举个例子，

```java
 $("div").filter("#div1").css('background-color', 'blue'); 
```

匹配所有 div 元素，并选择包含 id“div 1”的 div 元素，并将元素的背景色更改为蓝色。

 ## 2.过滤器(功能)

在一组匹配的元素中，获取通过函数测试的元素。该函数在内部传递一个索引参数，该参数表示匹配元素的索引，从 0 开始计数。

举个例子，

```java
 $('div').filter(function(index) {
	if(index==2 || index==3){ //0 index based
		return true;
	}
}).css('background-color', 'blue'); 
```

匹配所有 div 元素，使用函数过滤，只选择第三(2)和第四(3)个 div 元素。

```java
 $('div').filter(function(index) {
	return $("b", this).length == 1;
}).css('background-color', 'blue'); 
```

匹配所有 div 元素，用函数过滤选择包含**标签的 div 元素。**

 ## jQuery filter()示例

```java
 <html>
<head>
<title>jQuery filter example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>

<body>

<h1>jQuery filter example</h1>

<script type="text/javascript">

  $(document).ready(function(){

    $("#filterSelector").click(function () {

	$('div').css('background-color', 'white');

	$("div").filter("#div1").css('background-color', 'blue');

    });

    $("#filterFunction").click(function () {

	$('div').css('background-color', 'white');

	$('div').filter(function(index) {
		if(index==2 || index==3){ //0 index based
			return true;
		}
	}).css('background-color', 'blue');

    });

    $("#filterFunction2").click(function () {

	$('div').css('background-color', 'white');

	$('div').filter(function(index) {
		return $("b", this).length == 1;
	}).css('background-color', 'blue');

    });

  });
</script>
</head><body>

<div id="div1">
	<b>This is div 1 with 'b' tag</b>
</div>
<div id="div2">
	This is div 2
</div>
<div id="div3">
	<b>This is div 3 with 'b' tag</b>
</div>
<div id="div4">
	This is div 4
</div>

<br/>
<br/>
<br/>

<input type='button' value='filter(selector)' id='filterSelector'>
<input type='button' value='filter(index)' id='filterFunction'>
<input type='button' value='filter(index)+b' id='filterFunction2'>
</body>
</html> 
```

[http://web.archive.org/web/20190306103218if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-filter-example.html](http://web.archive.org/web/20190306103218if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-filter-example.html)

[Try Demo](http://web.archive.org/web/20190306103218/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-filter-example.html)[jquery](http://web.archive.org/web/20190306103218/http://www.mkyong.com/tag/jquery/) [jquery traversing](http://web.archive.org/web/20190306103218/http://www.mkyong.com/tag/jquery-traversing/)







