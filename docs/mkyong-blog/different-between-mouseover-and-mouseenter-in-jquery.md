# jQuery 中的 mouseover()和 mouseenter()不同

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/different-between-mouseover-and-mouseenter-in-jquery/>

在 jQuery 中，当鼠标进入匹配的元素时，触发 **mouseover()** 和 **mouseenter()** 事件。唯一不同的是在子元素中处理“**事件冒泡**的方式，让我们看两个场景:

## 1.没有子元素

如果匹配的元素没有子元素，那么 **mouseover()** 和 **mouseenter()** 事件的工作方式完全相同。参见下面的“亲自尝试”。

 ## 2.内部有子元素

如果匹配的元素有子元素，那么 **mouseover()** 和 **mouseenter()** 事件的工作方式是不同的，就是“**事件冒泡**”:

例如，“outerBox”包含一个子元素“innerBox”。

```java
 <div id="outerBox">OuterBox
	<div id="innerBox">InnerBox
	</div>
</div> 
```

请确保外部盒子和内部盒子都与特定事件相关联。

##### 鼠标悬停()

1.  当鼠标进入“outerBox”时，触发“outerBox”事件。
2.  当鼠标进入“innerBox”时，触发“innerBox”事件，然后触发“outerBox”事件。
3.  当鼠标返回到“外部盒子”时，触发“外部盒子”事件。

##### 滑鼠 enter()

1.  当鼠标进入“outerBox”时，触发“outerBox”事件。
2.  当鼠标进入“innerBox”时，触发“innerBox”事件。
3.  当鼠标回到“外部盒子”时，不会触发任何事件。

雅，这是相当混乱和理解在“话”，让理解它通过播放下面的例子:

 ## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>

<style type="text/css">
	#mouseover-outerBox1, #mouseenter-outerBox1,
	#mouseover-outerBox2, #mouseenter-outerBox2{
		margin:8px;
		border:1px groove #999966;
		background-color : #999966;
		width:150px;
		height:150px;
		color:white;
	}
	#mouseover-innerBox2, #mouseenter-innerBox2{
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
  <h1>jQuery mouseover() vs mouseenter() example</h1>

<div class="content">
  <div class="container1">
	  <span>mouseover() - no child element</span>
	  <div id="mouseover-outerBox1">OuterBox
	  </div>
	  <span id="mouseover-msg1">#mouseover is fired : 0</span>
  </div>

  <div class="container1">
  	  <span>mouseenter() - no child element</span>
	  <div id="mouseenter-outerBox1">OuterBox
	  </div>
	  <span id="mouseenter-msg1">#mouseenter is fired : 0</span>
  </div>
</div>

<div class="content">
  <div class="container1">
	  <span>mouseover() - with child elements</span>
	  <div id="mouseover-outerBox2">OuterBox
	  	<div id="mouseover-innerBox2">InnerBox
	  	</div>
	  </div>
	  <span id="mouseover-outer-msg2">#mouseover outer is fired : 0</span>
          <br/>
	  <span id="mouseover-inner-msg2">#mouseover inner is fired : 0</span>
  </div>

  <div class="container1">
  	  <span>mouseenter() - with child elements</span>
	  <div id="mouseenter-outerBox2">OuterBox
	  	<div id="mouseenter-innerBox2">InnerBox
	  	</div>
	  </div>
	  <span id="mouseenter-outer-msg2">#mouseenter outer is fired : 0</span>
          <br/>
	  <span id="mouseenter-inner-msg2">#mouseenter inner is fired : 0</span>
  </div>
</div>

<script type="text/javascript">

//example 1
var mouseover1=1;
$('#mouseover-outerBox1').mouseover(function(event) {
  $('#mouseover-msg1').text('#mouseover is fired : ' + mouseover1++)
});

var mouseenter1=1;
$('#mouseenter-outerBox1').mouseenter(function(event) {
  $('#mouseenter-msg1').text('#mouseenter is fired : ' + mouseenter1++)
});

//example 2
var mouseoverouter2=1;
$('#mouseover-outerBox2').mouseover(function(event) {
  $('#mouseover-outer-msg2')
              .text('#mouseover outer is fired : ' + mouseoverouter2++)
});

var mouseoverinner2=1;
$('#mouseover-innerBox2').mouseover(function(event) {
  $('#mouseover-inner-msg2')
              .text('#mouseover inner is fired : ' + mouseoverinner2++)
});

var mouseenterouter2=1;
$('#mouseenter-outerBox2').mouseenter(function(event) {
  $('#mouseenter-outer-msg2')
              .text('#mouseenter outer is fired : ' + mouseenterouter2++)
});

var mouseenterinner2=1;
$('#mouseenter-innerBox2').mouseenter(function(event) {
  $('#mouseenter-inner-msg2')
               .text('#mouseenter inner is fired : ' + mouseenterinner2++)
});

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190214231107if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseover-mouseenter-example.html](http://web.archive.org/web/20190214231107if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseover-mouseenter-example.html)

[Try Demo](http://web.archive.org/web/20190214231107/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-mouseover-mouseenter-example.html)[jquery](http://web.archive.org/web/20190214231107/http://www.mkyong.com/tag/jquery/) [mouse movement](http://web.archive.org/web/20190214231107/http://www.mkyong.com/tag/mouse-movement/)







