# jQuery prev()示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-prev-example/>

prev()函数用于获取匹配元素集中的前一个兄弟元素。只有前一个同级的元素被选择，它的子元素将被忽略。

这个 prev()函数允许通过“选择器”对其进行过滤。例如，prev('div ')用于获取仅是

元素的前一个兄弟元素。

## jQuery prev()示例

```java
 <html>
<head>
<style type="text/css">
  div,p { 
  	width:110px; 
	height:40px; 
	margin:2px 8px 2px 8px;
        float:left; 
	border:1px blue solid; 
  }
 </style>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
</head>
<body>
  <h1>jQuery prev() example</h1>

  <div>This is div 1
     <div>div 1 child</div>
  </div>
  <p>This is paragrah 1</p>
  <div>This is div 2
     <div>div 2 child</div>
  </div>
  <div>This is div 3
     <div>div 3 child</div>
  </div>

  <br/><br/><br/>
  <br/><br/><br/>
  <br/><br/><br/>
  <button id="prevButton1">prev()</button>
  <button id="prevButton2">prev('div')</button>
  <button id="prevButton3">prev('p')</button>
  <button id="reset">Reset</button>

<script type="text/javascript">

    var $currElement = $("#start");
    $currElement.css("background", "red");

    $("#prevButton1").click(function () {

	  if(!$currElement.prev().length){
	  	alert("No element found!");
		return false;	
	  }

	  $currElement = $currElement.prev();

      $("div,p").css("background", "");
      $currElement.css("background", "red");
    });

    $("#prevButton2").click(function () {

	  if(!$currElement.prev('div').length){
	  	alert("No element found!");
		return false;	
	  }

	  $currElement = $currElement.prev('div');

      $("div,p").css("background", "");
      $currElement.css("background", "red");
    });

    $("#prevButton3").click(function () {

	  if(!$currElement.prev('p').length){
	  	alert("No element found!");
		return false;	
	  }

	  $currElement = $currElement.prev('p');

      $("div,p").css("background", "");
      $currElement.css("background", "red");
    });

    $("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190214224036if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-prev-example.html](http://web.archive.org/web/20190214224036if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-prev-example.html)

[Try Demo](http://web.archive.org/web/20190214224036/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-prev-example.html)[jquery](http://web.archive.org/web/20190214224036/http://www.mkyong.com/tag/jquery/) [jquery traversing](http://web.archive.org/web/20190214224036/http://www.mkyong.com/tag/jquery-traversing/)![](img/b91584fcc5d1b2ecd274fa426d8326cd.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214224036/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5099">







