# jQuery text()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-text-example/>

jQuery **text()** 用于获取所有匹配元素的内容；而**text(‘新文本’)**用于替换或设置所有匹配元素的文本内容。

举个例子，

```java
 <div class="TClass">TEXT 1</div>
<div class="Tlass">TEXT 2</div> 
```

## 1\. $(‘.TClass’).text()

这将组合所有匹配元素的内容，得到“ **TEXT 1 TEXT 2** ”作为返回。与 html()不同，只获取第一个元素的内容。

## 2.$('.t class’)。文本('**这是新文本【T1 ')**

所有 html 标签 **<和>将被替换为&lt；以及&gt；**，将不应用 html 效果。

```java
 <div class="AClass"><b>This is new text</b></div>
<div class="AClass"><b>This is new text</b></div> 
```

## jQuery text()示例

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.textClass{
		padding:8px;
		border:1px solid blue;
		margin-bottom:8px;
	}
</style>

</head>
<body>
  <h1>jQuery text() example</h1>

  <div class="textClass">Text ABC ....</div>

  <div class="textClass">Text ABC 2....</div>

  <p>
  <button id="getText">text()</button>
  <button id="setText">text('xxx')</button>
  <button id="reset">reset</button>
  </p>
<script type="text/javascript">

    $("#getText").click(function () {

	  alert($('.textClass').text());

    });

    $("#setText").click(function () {

	  $('.textClass').text('<b>This is a new text</b>');

    });

    $("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-text-example.html](http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-text-example.html)

[Try Demo](http://web.archive.org/web/20220826132030/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-text-example.html)<input type="hidden" id="mkyong-current-postId" value="5132">