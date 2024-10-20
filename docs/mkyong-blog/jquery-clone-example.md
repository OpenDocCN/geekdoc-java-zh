# jQuery clone()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-clone-example/>

jQuery **clone()** 用于创建匹配元素的副本。它还支持一个布尔参数来指示是否需要将事件处理程序和数据与匹配的元素一起复制。

## 1.克隆 html 元素

例如，您将克隆以下 html 代码。

```java
 <div class="smallBox">
	I'm a small box
	<div class="smallInnerBox">I'm a small small inner box</div>
 </div> 
```

使用 clone()创建上述元素的副本，并将复制的元素放在包含类名“smallBox”的 div 标签之后。

```java
 $('.smallBox').clone().insertAfter(".smallBox"); 
```

结果是:

```java
 <div class="smallBox">
	I'm a small box
	<div class="smallInnerBox">I'm a small small inner box</div>
 </div>
  <div class="smallBox">
	I'm a small box
	<div class="smallInnerBox">I'm a small small inner box</div>
 </div> 
```

## 2.克隆事件处理程序

下一个示例将是克隆按钮单击事件，您将复制包含 id“clone button 1”的按钮。

```java
 <button id="cloneButton1">clone()</button>

<script type="text/javascript">
    $("#cloneButton1").click(function () {

	  $('.smallBox').clone().insertAfter(".smallBox");

    });
</script> 
```

如果使用默认的 **clone()或 clone(false)方法，它将只复制按钮元素，而不复制 click()事件处理程序。**

```java
 $('#cloneButton1').clone().insertAfter("#cloneButton1"); 
```

要复制 click()事件处理程序以及匹配的元素，应该使用 clone(true)。

```java
 $('#cloneButton1').clone(true).insertAfter("#cloneButton1"); 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	.smallBox{
		padding:8px;
		border:1px solid blue;
		margin:8px;
	}
	.smallInnerBox{
		padding:8px;
		border:1px solid green;
		margin:8px;
	}
</style>

</head>
<body>
  <h1>jQuery clone() example</h1>

  <div class="smallBox">
	I'm a small box
	<div class="smallInnerBox">I'm a small small inner box</div>
   </div>

  <p>
  <button id="cloneButton1">clone()</button>
  <button id="cloneButton2">clone() button (default)</button>
  <button id="cloneButton3">clone(true) button</button>
  <button id="reset">reset</button>
  </p>

<script type="text/javascript">

    $("#reset").click(function () {
	  location.reload();
    });

    $("#cloneButton1").click(function () {

	  $('.smallBox').clone().insertAfter(".smallBox");

    });

    $("#cloneButton2").click(function () {

	  $('#cloneButton1').clone(false).insertAfter("#cloneButton1");

    });

    $("#cloneButton3").click(function () {

	  $('#cloneButton1').clone(true).insertAfter("#cloneButton1");

    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-clone-example.html](http://web.archive.org/web/20220826132030if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-clone-example.html)

[Try Demo](http://web.archive.org/web/20220826132030/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-clone-example.html)<input type="hidden" id="mkyong-current-postId" value="5159">