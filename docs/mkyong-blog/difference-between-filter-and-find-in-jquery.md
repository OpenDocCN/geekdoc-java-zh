# jQuery 中 filter()和 find()的区别

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/difference-between-filter-and-find-in-jquery/>

[filter()](http://web.archive.org/web/20200616172522/http://www.mkyong.com/jquery/jquery-filter-example/) 和 [find()](http://web.archive.org/web/20200616172522/http://www.mkyong.com/jquery/jquery-find-example/) 方法非常相似，只是前者适用于所有元素，而后者只搜索子元素。

太简单了

1.  filter()–搜索所有元素。
2.  find()–仅搜索所有子元素。

## jQuery filter()与 find()示例

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

<h1>jQuery find() vs filter() example</h1>

<script type="text/javascript">

  $(document).ready(function(){

    $("#filterClick").click(function () {

	$('div').css('background','white');

	$('div').filter('#Fruits').css('background','red');

    });

    $("#findClick").click(function () {

	$('div').css('background','white');

	$('div').find('#Fruits').css('background','red');

    });

  });
</script>
</head><body>

<div id="Fruits">
	Fruits
	<div id="Apple">Apple</div>
	<div id="Banana">Banana</div>
</div>

<div id="Category">
	Category
	<div id="Fruits">Fruits</div>
	<div id="Animals">Animals</div>
</div>

<br/>
<br/>
<br/>

<input type='button' value='filter(Fruits)' id='filterClick'>
<input type='button' value='find(Fruits)' id='findClick'>

</body>
</html> 
```

[http://web.archive.org/web/20200616172522if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-filter-example.html](http://web.archive.org/web/20200616172522if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-filter-example.html)

[Try Demo](http://web.archive.org/web/20200616172522/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-find-filter-example.html)Tags : [jquery](http://web.archive.org/web/20200616172522/https://mkyong.com/tag/jquery/) [jquery traversing](http://web.archive.org/web/20200616172522/https://mkyong.com/tag/jquery-traversing/)<input type="hidden" id="mkyong-current-postId" value="5096">