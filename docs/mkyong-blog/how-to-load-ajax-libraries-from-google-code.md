# 如何从 Google 代码加载 Ajax 库？

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-load-ajax-libraries-from-google-code/>

谷歌代码托管了很多 Ajax 库，可以从任何服务器或网站免费下载。这里是迄今为止提供的 Ajax 库列表。

1.  jQuery
2.  jQuery UI
3.  原型
4.  script.aculo.us
5.  MooTools
6.  柔道训练学校
7.  雅虎！用户界面库(YUI)

## 为什么需要从 Google 代码中加载 Ajax 库？

有时候很多人会问为什么要从谷歌代码中加载 Ajax 库？基本上，有两个明显的好处:

1.  节省带宽——谷歌服务器比你的服务器快，从谷歌代码库加载非常快。
2.  缓存——如果用户访问了一些使用谷歌代码的网站，谷歌 Ajax 库很有可能被缓存。这将增加您的网站响应时间。

从 Google 代码加载 JQuery 库的例子。

```java
 <script src="http://www.google.com/jsapi"></script>  
<script type="text/javascript">  

    google.load("jquery", "1.2.6");  

    google.setOnLoadCallback(function() {  
          //your code
    });  

</script> 
```

**或者**，你也可以像这样包含一个直接链接

```java
 <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.2.6/jquery.min.js"
type="text/javascript"></script> 
```

 ## 参考

1.  [http://code . Google . com/APIs/Ajax libs/documentation/index . html](http://web.archive.org/web/20190304032337/http://code.google.com/apis/ajaxlibs/documentation/index.html)

[ajax](http://web.archive.org/web/20190304032337/http://www.mkyong.com/tag/ajax/) [jquery](http://web.archive.org/web/20190304032337/http://www.mkyong.com/tag/jquery/)![](img/77a433a2c8fcbda766aefd14fd7e4a5a.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190304032337/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="536">







