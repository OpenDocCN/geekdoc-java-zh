# jQuery 中 mouseout()和 mouseleave()之间的不同

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/different-between-mouseout-and-mouseleave-in-jquery/>

在 jQuery 中，当鼠标离开匹配的元素时，触发 **mouseout()** 和 **mouseleave()** 事件。唯一不同的是在子元素中处理“**事件冒泡**的方式，让我们看两个场景:

## 1.没有子元素

如果匹配的元素没有子元素，那么 **mouseout()** 和 **mouseleave()** 事件的工作方式完全相同。参见下面的“亲自尝试”。

 ## 2.内部有子元素

如果匹配的元素有子元素，那么 **mouseout()** 和 **mouseleave()** 事件的工作方式是不同的，其方式是“**事件冒泡**”:

例如，“outerBox”包含一个子元素“innerBox”。

```java
 <div id="outerBox">OuterBox
	<div id="innerBox">InnerBox
	</div>
</div> 
```

请确保外部盒子和内部盒子都与特定事件相关联。

##### mouseout()

1.  当鼠标进入“外部盒子”时，不会触发任何事件。
2.  当鼠标离开“外部盒子”并进入“内部盒子”时，触发“外部盒子”事件。
3.  当鼠标离开“innerBox”并进入“outerBox”时，触发“innerBox”事件，然后触发“outerBox”事件。
4.  当鼠标离开“outerBox”时，触发“outerBox”事件。

##### 鼠标离开()

1.  当鼠标进入“外部盒子”时，不会触发任何事件。
2.  当鼠标离开“外部盒子”并进入“内部盒子”时，不会触发任何事件。
3.  当鼠标离开“内盒”并进入“外盒”时，触发“内盒”事件。
4.  当鼠标离开“outerBox”时，触发“outerBox”事件。

让我们通过播放下面的例子来理解它:

 ## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#mouseout-outerBox1, #mouseleave-outerBox1,
	#mouseout-outerBox2, #mouseleave-outerBox2{
		margin:8px;
		border:1px groove #999966;
		background-color : #999966;
		width:150px;
		height:150px;
		color:white;
	}
	#mouseout-innerBox2, #mouseleave-innerBox2{
		margin:8px 8px 8px 16px;
		border:1px groove #0000FF;
		background-color : #0000FF;
		width:100px;
		height:100px;
		color:white;
	}
	span{
		padding:8px;
	}
	.content{
		width:500px;
		height:250px;
	}
	.container1{
		float:left;
		padding-right:16px;
	}
</style>

</head>
<body>
  <h1>jQuery mouseout() vs mouseleave() example</h1>

<div class="content">
  <div class="container1">
	  <span>mouseout() - no child element</span>
	  <div id="mouseout-outerBox1">OuterBox
	  </div>
	  <span id="mouseout-msg1">#mouseout is fired : 0</span>
  </div>

  <div class="container1">
  	  <span>mouseleave() - no child element</span>
	  <div id="mouseleave-outerBox1">OuterBox
	  </div>
	  <span id="mouseleave-msg1">#mouseleave is fired : 0</span>
  </div>
</div>

<div class="content">
  <div class="container1">
	  <span>mouseout() - with child elements</span>
	  <div id="mouseout-outerBox2">OuterBox
	  	<div id="mouseout-innerBox2">InnerBox
	  	</div>
	  </div>
	  <span id="mouseout-outer-msg2">#mouseout outer is fired : 0</span>
          <br/>
	  <span id="mouseout-inner-msg2">#mouseout inner is fired : 0</span>
  </div>

  <div class="container1">
  	  <span>mouseleave() - with child elements</span>
	  <div id="mouseleave-outerBox2">OuterBox
	  	<div id="mouseleave-innerBox2">InnerBox
	  	</div>
	  </div>
	  <span id="mouseleave-outer-msg2">#mouseleave outer is fired : 0</span>
          <br/>
	  <span id="mouseleave-inner-msg2">#mouseleave inner is fired : 0</span>
  </div>
</div>

<script type="text/javascript">

//example 1
var mouseout1=1;
$('#mouseout-outerBox1').mouseout(function(event) {
  $('#mouseout-msg1').text('#mouseout is fired : ' + mouseout1++)
});

var mouseleave1=1;
$('#mouseleave-outerBox1').mouseleave(function(event) {
  $('#mouseleave-msg1').text('#mouseleave is fired : ' + mouseleave1++)
});

//example 2
var mouseoutouter2=1;
$('#mouseout-outerBox2').mouseout(function(event) {
  $('#mouseout-outer-msg2').text('#mouseout outer is fired : ' + mouseoutouter2++)
});

var mouseoutinner2=1;
$('#mouseout-innerBox2').mouseout(function(event) {
  $('#mouseout-inner-msg2').text('#mouseout inner is fired : ' + mouseoutinner2++)
});

var mouseleaveouter2=1;
$('#mouseleave-outerBox2').mouseleave(function(event) {
  $('#mouseleave-outer-msg2')
         .text('#mouseleave outer is fired : ' + mouseleaveouter2++)
});

var mouseleaveinner2=1;
$('#mouseleave-innerBox2').mouseleave(function(event) {
  $('#mouseleave-inner-msg2')
         .text('#mouseleave inner is fired : ' + mouseleaveinner2++)
});

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190227120536if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseout-mouseleave-example.html](http://web.archive.org/web/20190227120536if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseout-mouseleave-example.html)

[Try Demo](http://web.archive.org/web/20190227120536/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseout-mouseleave-example.html)[jquery](http://web.archive.org/web/20190227120536/http://www.mkyong.com/tag/jquery/) [mouse event](http://web.archive.org/web/20190227120536/http://www.mkyong.com/tag/mouse-event/)







