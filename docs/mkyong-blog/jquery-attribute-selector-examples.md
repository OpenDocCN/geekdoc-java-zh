# jQuery–属性选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-attribute-selector-examples/>

在 jQuery 中，属性选择器包含在方括号[]中。以下是支持的属性选择器:

## 1.具有属性[A]

选择所有具有“A”属性的元素。

**示例**
$(' a[rel]')–选择与具有 rel 属性的< a >匹配的所有元素。

 ## 2.属性值等于[A=B]

选择 A 属性的值恰好等于 b 的所有元素。

**示例**
$(' a[rel = nofollow]')–选择与< a >匹配的所有元素，这些元素的 rel 属性值正好等于' no follow '。

 ## 3.属性值不等于[A！=B]

选择所有不具有值正好等于 b 的 A 属性的元素。

**例句**
$('a[rel！= nofollow]')–选择与< a >匹配的所有元素，这些元素没有值正好等于“no follow”的 rel 属性。

## 4.属性值以[A^=B]开头

选择所有 A 属性的值以 b 开头的元素。

**示例**
$('a[rel^=nof]')–选择与< a >匹配的所有元素，这些元素的 rel 属性的值以“nof”开头。

## 5.属性值结束[A$=B]

选择所有具有 A 属性且值以 b 结尾的元素。

**示例**
$(' a[rel $ = low]')–选择与< a >匹配的所有元素，这些元素的 rel 属性值以' low '结尾。

## 6.属性值包含[A*=B]

选择所有具有 A 属性的元素，该属性的值包含子字符串 b。

**示例**
$(' a[href * = yahoo.com]')–选择与< a >匹配的所有元素，这些元素的 href 属性值包含' Yahoo . com '。

## 7.属性值包含前缀[A|=B]

选择 A 属性值等于 B 或以 B 开头后跟连字符(-)的所有元素。

**示例**
$(' a[lang | = en]')–选择与< a >匹配的所有元素，这些元素的阿郎属性值等于' en '或' en-'。

## 8.属性值包含–由空格[A~=B]分隔

选择所有 A 属性的值等于 B 并以空格分隔的元素。

**示例**
$(' div[class ~ = jQuery]')–选择与< div >匹配的所有元素，这些元素的 class 属性值等于' jQuery '，用空格分隔。例如，“HellojQuery”匹配，“Hello-jQuery”和“Hello jQuery”不匹配。

## 播放它

点击按钮来玩属性选择器。

```java
 <html>
<head>
<title>jQuery attribute selector example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div, a{
		padding:16px;
	}

	#msg{
		padding:8px;
		hright:100px;
	}
</style>
</head>

<body>

<h1>jQuery attribute selector example</h1>

<div id="msg"></div>

<div>
    <a rel="nofollow" href="http://www.google.com" lang="en-US">
    Google - <a rel="nofollow" href="http://www.google.com" lang="en-US">
    </a>
</div>

<div>
    <a href="http://www.yahoo.com" lang="en">
     Yahoo - <a href="http://www.yahoo.com" lang="en" >
    </a>
</div>

<div>
    <a href="http://www.abc-yahoo.com" lang="-en">
     Yahoo - <a href="http://www.abc-yahoo.com" lang="-en">
    </a>
</div>

<div class="Hello-jQuery">
	class = "Hello-jQuery"
</div>

<div class="Hello jQuery">
	class = "Hello jQuery"
</div>

<div class="HellojQuery">
	class = "HellojQuery"
</div>

<br/><br/>
<button>a[rel]</button>
<button>a[rel=nofollow]</button>
<button>a[rel!=nofollow]</button>
<button>a[rel^=nof]</button>
<button>a[rel$=low]</button>

<button>a[href*=yahoo.com]</button>
<button>a[lang|=en]</button>
<button>div[class~=jQuery]</button>

<button id="reset">Reset It</button>

<script type="text/javascript">
    $("button").click(function () {

	  var str = $(this).text();	
	  $('a').css("border", "0px solid #000000");
	  $(str).css("border", "1px solid #ff0000");
	  $('#msg').html("<h2>Attribute Selector : " + str + "</h2>");
    });

	$("#reset").click(function () {
      location.reload();
    });

</script>

</body>
</html> 
```

[http://web.archive.org/web/20190304032448if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-attribute-selector.html](http://web.archive.org/web/20190304032448if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-attribute-selector.html)

[Try Demo](http://web.archive.org/web/20190304032448/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-attribute-selector.html)[jquery](http://web.archive.org/web/20190304032448/http://www.mkyong.com/tag/jquery/) [jquery attribute](http://web.archive.org/web/20190304032448/http://www.mkyong.com/tag/jquery-attribute/)







