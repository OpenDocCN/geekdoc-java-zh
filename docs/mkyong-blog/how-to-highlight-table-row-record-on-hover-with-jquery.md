# 如何用 jQuery 突出显示悬停时的表格行记录

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-highlight-table-row-record-on-hover-with-jquery/>

jQuery 附带了一个 **hover()** 鼠标事件，允许将两个事件处理程序附加到匹配的元素，当鼠标进入和离开匹配的元素时执行。

```java
 $("#id").hover(A, B); 
```

1.  当鼠标进入匹配的元素时调用的函数。
2.  b–当鼠标离开匹配的元素时调用的函数。

这是突出显示表格行记录的最佳功能。请参见 jQuery 代码片段:

```java
 $("tr").not(':first').hover(
  function () {
    $(this).css("background","yellow");
  }, 
  function () {
    $(this).css("background","");
  }
); 
```

它将在悬停时突出显示表格行记录，颜色为黄色。**。not(':first')** "是避免突出显示标题行记录的常见实现。

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.4.2.min.js"></script>
</head>
<body>
  <h1>Highlight table row record on hover - jQuery</h1>

  <table border="1">
    <tr><th>No</th><th>Name</th><th>Age</th><th>Salary</th></tr>
    <tr><td>1</td><td>Yong Mook Kim</td><td>28</td><td>$100,000</td></tr>
    <tr><td>2</td><td>Low Yin Fong</td><td>29</td><td>$90,000</td></tr>
    <tr><td>3</td><td>Ah Pig</td><td>18</td><td>$50,000</td></tr>
    <tr><td>4</td><td>Ah Dog</td><td>28</td><td>$40,000</td></tr>
    <tr><td>5</td><td>Ah Cat</td><td>28</td><td>$30,000</td></tr>
  </table>

<script type="text/javascript">

$("tr").not(':first').hover(
  function () {
    $(this).css("background","yellow");
  }, 
  function () {
    $(this).css("background","");
  }
);

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190214222705if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-highlight-table-row-record.html](http://web.archive.org/web/20190214222705if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-highlight-table-row-record.html)

[Try Demo](http://web.archive.org/web/20190214222705/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-highlight-table-row-record.html)[highlight](http://web.archive.org/web/20190214222705/http://www.mkyong.com/tag/highlight/) [jquery](http://web.archive.org/web/20190214222705/http://www.mkyong.com/tag/jquery/) [jquery effects](http://web.archive.org/web/20190214222705/http://www.mkyong.com/tag/jquery-effects/) [table](http://web.archive.org/web/20190214222705/http://www.mkyong.com/tag/table/)![](img/4fce08d192e1a9add0b2e4a94d2f0c20.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214222705/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5202">







