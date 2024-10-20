# jQuery bind()和 unbind()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-bind-and-unbind-example/>

jQuery **bind()** 函数用于将事件处理程序附加到元素上，而 **unbind()** 用于将现有的事件处理程序从元素上分离。

## 例子

演示用的简单 html 代码。

```java
 <div id="BoxId">
	Mouseover Me, Click Me or Double Click Me
</div>
<span></span> 
```

 ## 1.绑定()

jQuery 完全支持 JavaScript 事件类型，如“click”、“dblclick”或自定义事件名称。

将单击事件绑定到 Id 为“BoxId”的元素。

```java
 $("#BoxId").bind("click", (function () {
	$('span').text("Single Clicked");
})); 
```

将双击事件绑定到 Id 为“BoxId”的元素。

```java
 $("#BoxId").bind("dblclick", (function () {
	$('span').text("Double Clicked");
})); 
```

 ## 2.bind() +事件对象

jQuery 附带了许多事件对象，要获得关于用户环境的更多信息，请查看这里的 [jQuery 事件对象细节](http://web.archive.org/web/20190210095257/http://api.jquery.com/category/events/event-object/)。

将带有事件对象参数的“mouseover”事件绑定到 Id 为“BoxId”的元素。

```java
 $("#BoxId").bind("mouseover", (function (event) {
	$('span').text('The mouse cursor is at ('
	+ event.pageX + ', ' + event.pageY + ')');
})); 
```

## 3.bind() +事件数据

这意味着向 bind()函数传递一个自定义参数数据。

绑定一个单击事件，并将自定义消息作为参数传递给 Id 为“BoxId”的元素。在函数内部，您可以借助 **event.data** 访问参数信息。

```java
 var name = 'Message pass by jQuery event data';
$("#BoxId").bind("click", {msg: name},(function (event) {
	$('span').text("Single Clicked - " + event.data.msg);
})); 
```

## 4.bind() +多个事件

要将多个事件绑定在一起，您可以用空格分隔每个事件。

将单击和双击事件绑定到 Id 为“BoxId”的元素。

```java
 $("#BoxId").bind("click dblclick", (function () {
	$('span').text("Single Clicked");
})); 
```

或者，也可以像下面这样编码:(**在 jQuery 1.4** 中受支持)。

```java
 $("#BoxId").bind({
	click : function(){
		$('span').text("Single Clicked");
	},
	dblclick : function(){
		$('span').text("Double Clicked");
	}
}); 
```

## 5.解除绑定()

解除现有事件的绑定或分离相当容易，只需要指定附加的事件类型。

从 Id 为“BoxId”的元素中分离“click”和“dblclick”事件。

```java
 $('#BoxId').unbind("click");
$('#BoxId').unbind("dblclick"); 
```

## 你自己试试

```java
 <html>
<head>
<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	.ClassA{
		padding:8px;
		border:1px solid blue;
	}
	span{
		color:#CC6633;
	}
</style>

</head>
<body>
  <h1>jQuery bind() and unbind() example</h1>

  <div id="BoxId">
	Mouseover Me, Click Me or Double Click Me
  </div>
  <span></span>
  <p>
  <button id="bindButton">bind()</button>
  <button id="bindMessageButton">bind(message)</button>
  <button id="unbindButton">unbind()</button>
  </p>

<script type="text/javascript">

    $("#BoxId").bind("click", (function () {
		$('span').text("Single Clicked");
    }));

    $("#BoxId").bind("dblclick", (function () {
		$('span').text("Double Clicked");
    }));

   //Event object
   $("#BoxId").bind("mouseover", (function (event) {
		$('span').text('The mouse cursor is at ('
                 + event.pageX + ', ' + event.pageY + ')');
    }));

    //Passing event data
    $("#bindMessageButton").bind("click", (function () {
		var name = 'Message pass by jQuery event data';
		$("#BoxId").bind("click", {msg: name},(function (event) {
			$('span').text("Single Clicked - " + event.data.msg);
     }));
    }));

    $("#unbindButton").bind("click",(function () {
		$('#BoxId').unbind("click");
		$('#BoxId').unbind("dblclick");
		$('#BoxId').unbind("mouseover");
		$('span').text("");
    }));

    //added since version 1.4
    $("#bindButton").bind("click",(function () {

		$("#BoxId").bind({
			click : function(){
				$('span').text("Single Clicked");
			},
			dblclick : function(){
				$('span').text("Double Clicked");
			},
			mouseover : function(event){
				$('span').text('The mouse cursor is at ('
                                + event.pageX + ', ' + event.pageY + ')');
			}
		});
    }));

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190210095257if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-bind-unbind-example.html](http://web.archive.org/web/20190210095257if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-bind-unbind-example.html)

[Try Demo](http://web.archive.org/web/20190210095257/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-bind-unbind-example.html)[jquery](http://web.archive.org/web/20190210095257/http://www.mkyong.com/tag/jquery/) [jquery event handler](http://web.archive.org/web/20190210095257/http://www.mkyong.com/tag/jquery-event-handler/)







