# 如何用 jQuery 检查一个元素是否有某个类名

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-check-if-an-element-has-a-certain-class-name-with-jquery/>

jQuery 提供了两种方法来检查元素是否有特定的类名。这两种方法具有相同的功能。

1.  是('。类名’)
2.  hasClass('。类名’)

例如，检查 div 元素的类名是否为“redColor”。

## 1.是('。类名’)

```java
 $('div').is('.redColor') 
```

 ## 2.hasClass('。类名’)

```java
 $('div').hasClass('.redColor') 
```

 ## 例子

如果 div 元素的类名为“redColor ”,那么将其类更改为“blueColor”。

```java
 <html>
<head>
<style type="text/css">
  .redColor { 
  	background:red;
  }
  .blueColor { 
  	background:blue;
  }
 </style>
<script type="text/javascript" src="jquery-1.3.2.min.js"></script>
</head>
<body>
  <h1>jQuery check if an element has a certain class</h1>

  <div class="redColor">This is a div tag with class name of "redColor"</div>

  <p>
  <button id="isTest">is('.redColor')</button>
  <button id="hasClassTest">hasClass('.redColor')</button>
  <button id="reset">reset</button>
  </p>
<script type="text/javascript">

    $("#isTest").click(function () {

	  if($('div').is('.redColor')){
	  	$('div').addClass('blueColor');
	  }

    });

    $("#hasClassTest").click(function () {

	  if($('div').hasClass('.redColor')){
	  	$('div').addClass('blueColor');
	  }

    });

	$("#reset").click(function () {
	  location.reload();
    });

</script>
</body>
</html> 
```

[http://web.archive.org/web/20190310101146if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-has-css-class.html](http://web.archive.org/web/20190310101146if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-has-css-class.html)

[Try Demo](http://web.archive.org/web/20190310101146/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-check-has-css-class.html)[jquery](http://web.archive.org/web/20190310101146/http://www.mkyong.com/tag/jquery/)







