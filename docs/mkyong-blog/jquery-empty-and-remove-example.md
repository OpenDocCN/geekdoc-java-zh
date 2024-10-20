# jQuery empty()和 remove()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-empty-and-remove-example/>

jQuery **empty()** 和 **remove()** 都是用来移除匹配的元素，只是前者是用来移除匹配元素的子元素；而后者用于完全移除所有匹配的元素。

## 例子

2 个 div 标签–“BBox”是“ABox”的子元素。

```java
 <div class="ABox">
	I'm a A box
	<div class="BBox">I'm a B box</div>
</div> 
```

## 1.空()

empty()将只移除“ABox”内容和“BBox”子元素。

```java
 $('.ABox').empty(); 
```

新的结果将是:

```java
 <div class="ABox">
</div> 
```

## 2.移除()

remove()将完全删除所有“ABox”和“BBox”元素。

```java
 $('.ABox').remove(); 
```

新的结果将是:

```java
 //nothing left 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.ABox{
		padding:8px;
		border:1px solid blue;
	}
	.BBox{
		padding:8px;
		border:1px solid red;
	}
</style>

</head>
<body>
  <h1>jQuery empty() and remove() example</h1>

  <div class="ABox">
	I'm a A box
	<div class="BBox">I'm a B box</div>
   </div>

  <p>
  <button id="empty">empty()</button>
  <button id="remove">remove()</button>
  <button id="reset">reset</button>
  </p>

<script type="text/javascript">

    $("#reset").click(function () {
	  location.reload();
    });

    $("#empty").click(function () {

	  $('.ABox').empty();

    });

    $("#remove").click(function () {

	  $('.ABox').remove();

    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132007if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-empty-remove-example.html](http://web.archive.org/web/20220826132007if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-empty-remove-example.html)

[Try Demo](http://web.archive.org/web/20220826132007/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-empty-remove-example.html)<input type="hidden" id="mkyong-current-postId" value="5164">