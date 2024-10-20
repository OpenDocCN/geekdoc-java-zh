# 如何用 jQuery 创建工具提示

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-create-a-tooltips-with-jquery/>

这里有一个用 jQuery 创建工具提示消息的简单想法。

## 想法…

1.  确定需要显示工具提示信息的“**目标**”。
2.  为它创建一个工具提示消息和 CSS 样式。
3.  三个功能，-**显示工具提示**，**隐藏工具提示**，**改变工具提示位置**。
4.  当鼠标进入**目标**时，使用**显示工具提示**显示工具提示信息，并用**改变工具提示位置**初始化位置。
5.  当鼠标在“**目标**周围移动时，用**changetooltipposition**不断改变工具提示信息的位置。
6.  当鼠标离开“目标”时，使用 hideTooltips 隐藏工具提示消息。
7.  使用 jQuery 来完成🙂

## 1.目标

“提示”和“用户名标签”是显示工具提示消息的目标。

```java
 <label id="username">Username : </label><input type="text" / size="50"> 
<span id="hint">hint (mouseover me)</span> 
```

## 2.工具提示 CSS

为工具提示消息创建 CSS 样式。

```java
 .tooltip{
	margin:8px;
	padding:8px;
	border:1px solid blue;
	background-color:yellow;
	position: absolute;
	z-index: 2;
} 
```

## 3.显示工具提示

将工具提示消息附加到“body”标签上，并在工具提示消息的位置上签名。

```java
 var showTooltip = function(event) {
   $('div.tooltip').remove();
   $('<div class="tooltip">I\' am tooltips! tooltips! tooltips! :)</div>')
     .appendTo('body');
   changeTooltipPosition(event);
}; 
```

## 5.更改工具提示

更改工具提示消息的位置。

```java
 var changeTooltipPosition = function(event) {
	var tooltipX = event.pageX - 8;
	var tooltipY = event.pageY + 8;
	$('div.tooltip').css({top: tooltipY, left: tooltipX});
}; 
```

## 6.隐藏工具提示

隐藏工具提示消息。

```java
 var hideTooltip = function() {
	$('div.tooltip').remove();
}; 
```

## 7.捆绑它

将鼠标事件绑定到目标。

```java
 $("span#hint,label#username'").bind({
	mousemove : changeTooltipPosition,
	mouseenter : showTooltip,
	mouseleave: hideTooltip
}); 
```

## 你自己试试

在本例中，将鼠标放在提示或标签上以显示工具提示效果。

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#hint{
		cursor:pointer;
	}
	.tooltip{
		margin:8px;
		padding:8px;
		border:1px solid blue;
		background-color:yellow;
		position: absolute;
		z-index: 2;
	}
</style>

</head>

<body>

<h1>jQuery tooltips example</h1>

<label id="username">Username : </label><input type="text" / size="50"> 
<span id="hint">hint (mouseover me)</span>

<script type="text/javascript">

$(document).ready(function() {

	var changeTooltipPosition = function(event) {
	  var tooltipX = event.pageX - 8;
	  var tooltipY = event.pageY + 8;
	  $('div.tooltip').css({top: tooltipY, left: tooltipX});
	};

	var showTooltip = function(event) {
	  $('div.tooltip').remove();
	  $('<div class="tooltip">I\' am tooltips! tooltips! tooltips! :)</div>')
            .appendTo('body');
	  changeTooltipPosition(event);
	};

	var hideTooltip = function() {
	   $('div.tooltip').remove();
	};

	$("span#hint,label#username'").bind({
	   mousemove : changeTooltipPosition,
	   mouseenter : showTooltip,
	   mouseleave: hideTooltip
	});
});

</script>

</body>
</html> 
```

[http://web.archive.org/web/20220118011614if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-tooltips-example.html](http://web.archive.org/web/20220118011614if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-tooltips-example.html)

[Try Demo](http://web.archive.org/web/20220118011614/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-tooltips-example.html)<input type="hidden" id="mkyong-current-postId" value="5280">