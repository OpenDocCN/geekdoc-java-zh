# jQuery html()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-html-example/>

jQuery **html()** 用于获取匹配元素的第一个元素的 html 内容；而**html(‘新 html 内容’)**用于替换或设置所有匹配元素的 html 内容。

例如，包含相同类名“AClass”的两个 div 元素。

```java
 <div class="AClass">ABC 1</div>
<div class="AClass">ABC 2</div> 
```

## 1.$('.a 类)。html()

这将只得到“ **ABC 1** ”，第二个匹配的元素“ABC 2”将被忽略。

## 2.$('.a 类)。html(' **这是新文本【T1 ')**

这将替换所有匹配元素的 html 内容。

```java
 <div class="AClass"><b>This is new text</b></div>
<div class="AClass"><b>This is new text</b></div> 
```

## jQuery html()示例

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.htmlClass{
		padding:8px;
		border:1px solid blue;
		margin-bottom:8px;
	}
</style>

</head>
<body>
  <h1>jQuery html() example</h1>

  <div class="htmlClass">I'm going to replate by something ....</div>

  <div class="htmlClass">I'm going to replate by something 2....</div>

  <p>
  <button id="getHtml">html()</button>
  <button id="setHtml">html('xxx')</button>
  <button id="reset">reset</button>
  </p>

  <script type="text/javascript">

    $("#getHtml").click(function () {

	  alert($('.htmlClass').html());

    });

    $("#setHtml").click(function () {

	  $('.htmlClass').html('<b>This is a new text</b>');

    });

    $("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-html-example.html](http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-html-example.html)

[Try Demo](http://web.archive.org/web/20220826132030/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-html-example.html)<input type="hidden" id="mkyong-current-postId" value="5129">