# 如何使用 jQuery 选择单选按钮

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-select-a-radio-button-with-jquery/>

一个用 jQuery 动态选择单选按钮的简单例子。

一个单选按钮组，带有一个 name="sex "。

```java
 <input type="radio" name="sex" value="Male">Male</input>
<input type="radio" name="sex" value="Female">Female</input>
<input type="radio" name="sex" value="Unknown">Unknown</input> 
```

1.显示选定的单选按钮值。

```java
 $('input:radio[name=sex]:checked').val(); 
```

2.选择单选按钮(男性)。
**单选按钮是以 0 为基的，所以'男' = '0 '，'女' = '1 '，'未知' = '2 '。**

```java
 $('input:radio[name=sex]:nth(0)').attr('checked',true);
or
$('input:radio[name=sex]')[0].checked = true; 
```

3.选择单选按钮(阴)。

```java
 $('input:radio[name=sex]:nth(1)').attr('checked',true);
or
$('input:radio[name=sex]')[1].checked = true; 
```

4.选择单选按钮(未知)。

```java
 $('input:radio[name=sex]:nth(2)').attr('checked',true);
or
$('input:radio[name=sex]')[2].checked = true; 
```

5.重置选定的单选按钮。

```java
 $('input:radio[name=sex]').attr('checked',false); 
```

## jQuery 选择单选按钮示例

```java
 <html>
<head>
<title>jQuery select a radio button example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>

<body>

<h1>jQuery select a radio button example</h1>

<script type="text/javascript">

  $(document).ready(function(){

    $("#isSelect").click(function () {

	alert($('input:radio[name=sex]:checked').val());

    });

    $("#selectMale").click(function () {

	$('input:radio[name=sex]:nth(0)').attr('checked',true);
	//$('input:radio[name=sex]')[0].checked = true;

    });

    $("#selectFemale").click(function () {

	$('input:radio[name=sex]:nth(1)').attr('checked',true);
	//$('input:radio[name=sex]')[1].checked = true;

    });

    $("#selectUnknown").click(function () {

	$('input:radio[name=sex]:nth(2)').attr('checked',true);
	//$('input:radio[name=sex]')[2].checked = true;

    });

    $("#reset").click(function () {

	$('input:radio[name=sex]').attr('checked',false);

    });

  });
</script>
</head><body>

<input type="radio" name="sex" value="Male">Male</input>
<input type="radio" name="sex" value="Female">Female</input>
<input type="radio" name="sex" value="Unknown">Unknown</input>

<br/>
<br/>
<br/>

<input type='button' value='Display Selected' id='isSelect'>
<input type='button' value='Select Male' id='selectMale'>
<input type='button' value='Select Female' id='selectFemale'>
<input type='button' value='Select Unknown' id='selectUnknown'>
<input type='button' value='Reset' id='reset'>

</body>
</html> 
```

[http://web.archive.org/web/20190308050044if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-radio-button.html](http://web.archive.org/web/20190308050044if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-radio-button.html)

[Try Demo](http://web.archive.org/web/20190308050044/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-radio-button.html)[jquery](http://web.archive.org/web/20190308050044/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190308050044/http://www.mkyong.com/tag/jquery-selector/) [radio button](http://web.archive.org/web/20190308050044/http://www.mkyong.com/tag/radio-button/)![](img/a55673bb26148373448eaa4e3725e160.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190308050044/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="5047">







