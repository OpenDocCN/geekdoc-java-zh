# jQuery wrap()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-wrap-example/>

jQuery **wrap()** 用于将 HTML 元素包装在每个匹配的元素周围。

举个例子，

```java
 <div class="innerBox">Ip man vs Iron man</div>
<div class="innerBox">Ip man 2 vs Iron man 2</div> 
```

用包含类名“wrapBox”的 div 标记包装它。

```java
 $('.innerBox').wrap("<div class='wrapBox'></div>"); 
```

新内容将变成:

```java
 <div class='wrapBox'>
   <div class="innerBox">Ip man vs Iron man</div>
</div>

<div class='wrapBox'>
   <div class="innerBox">Ip man 2 vs Iron man 2</div>
</div> 
```

**警告**
不要用 html 内容换行

```java
 $('.innerBox').wrap("<div class='wrapBox'>TESTING</div>"); 
```

你希望有下文吗？

```java
 <div class='wrapBox'>TESTING
   <div class="innerBox">Ip man vs Iron man</div>
</div> 
```

不，结果将是完全替换。wrap 只支持 html 标签，不支持内容。

```java
 <div class='wrapBox'>TESTING</div> 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.innerBox{
		padding:8px;
		border:1px solid blue;
		margin:8px;
	}
	.wrapBox{
		padding:8px;
		border:1px solid red;
		margin:8px;
	}
</style>

</head>
<body>
  <h1>jQuery wrap() example</h1>

  <div class="innerBox">Ip man vs Iron man</div>

  <div class="innerBox">Ip man 2 vs Iron man 2</div>

  <p>
  <button id="wrapButton">wrap()</button>
  <button id="reset">reset</button>
  </p>

<script type="text/javascript">

    $("#wrapButton").click(function () {

	  $('.innerBox').wrap("<div class='wrapBox'></div>");

    });

	$("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132031if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-wrap-example.html](http://web.archive.org/web/20220826132031if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-wrap-example.html)

[Try Demo](http://web.archive.org/web/20220826132031/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-wrap-example.html)<input type="hidden" id="mkyong-current-postId" value="5155">