# 如何在 jQuery 中动态添加/删除 CSS 类

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-add-remove-css-class-dynamically-in-jquery/>

jQuery 自带 **addClass()** 和 **removeClass()** 来动态添加或移除 CSS 类。举个例子，

1.  $('#para1 ')。addClass(“突出显示”)；–向包含 id“para 1”的元素添加“highlight”CSS 类。
2.  $('#para1 ')。removeClass('高亮')；–从包含 id“para 1”的元素中删除“highlight”CSS 类。

## 例子

```java
 <html>
<head>
<style type="text/css">
  .highlight { 
  	background:green;
  }
 </style>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
</head>
<body>
  <h1>jQuery add / remove css class example</h1>

  <p id="para1">This is paragraph 1</p>
  <p>This is paragraph 2</p>
  <p>This is paragraph 3</p>
  <p>This is paragraph 4</p>

  <button id="addClass">Add highlight</button>
  <button id="removeClass">Remove highlight</button>

<script type="text/javascript">

    $("#addClass").click(function () {

	  $('#para1').addClass('highlight');

    });

    $("#removeClass").click(function () {

	  $('#para1').removeClass('highlight');

    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190224165922if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-add-remove-css-class.html](http://web.archive.org/web/20190224165922if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-add-remove-css-class.html)

[Try Demo](http://web.archive.org/web/20190224165922/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-add-remove-css-class.html)[css](http://web.archive.org/web/20190224165922/http://www.mkyong.com/tag/css/) [jquery](http://web.archive.org/web/20190224165922/http://www.mkyong.com/tag/jquery/)![](img/45d913df91de7e51df5769fe4794f25e.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224165922/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5110">







