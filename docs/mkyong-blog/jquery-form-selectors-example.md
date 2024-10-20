# jQuery 表单选择器示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-form-selectors-example/>

jQuery 附带了许多表单选择器，可以更容易、更有效地访问表单元素。下面是一个简单的 jQuery 表单选择器参考。

## 1\. TextBox – $(‘input:text’)

选择文本框

```java
 $('input:text'); 
```

获取文本框值

```java
 $('input:text').val(); 
```

设置文本框值

```java
 $('input:text').val("New Text"); 
```

 ## 2.密码-$('输入:密码')

要选择密码

```java
 $('input:password'); 
```

获取密码值

```java
 $('input:password').val(); 
```

要设置密码值

```java
 $('input:text').val("New Text"); 
```

 ## 3.textarea-$(' textarea ')

要选择文本区域

```java
 $('textarea'); 
```

获取 textarea 值

```java
 $('textarea').val(); 
```

要设置 textarea 值

```java
 $('textarea').val("New Text in Textarea"); 
```

## 4.单选按钮-$('输入:单选')

要选择单选按钮

```java
 $('input:radio'); 
```

获取选中的单选按钮选项

```java
 $('input:radio[name=radiobutton-name]:checked').val(); 
```

选择第一个单选按钮选项

```java
 $('input:radio[name=radiobutton-name]:nth(0)').attr('checked',true);
or
$('input:radio[name=radiobutton-name]')[0].checked = true; 
```

[更多详情…](http://web.archive.org/web/20190227163628/http://www.mkyong.com/jquery/how-to-select-a-radio-button-with-jquery/)

## 5.复选框-$('输入:复选框')

要选择复选框

```java
 $('input:checkbox'); 
```

检查复选框是否被选中

```java
 $('input:checkbox[name=checkboxname]').is(':checked'); 
```

要选中复选框

```java
 $('input:checkbox[name=checkboxname]').attr('checked',true); 
```

取消选中复选框

```java
 $('input:checkbox[name=checkboxname]').attr('checked',false); 
```

[更多详情…](http://web.archive.org/web/20190227163628/http://www.mkyong.com/jquery/how-to-select-a-radio-button-with-jquery/)

## 6.上传文件-$('输入:文件')

要选择文件

```java
 $('input:file'); 
```

获取文件值

```java
 $('input:file').val(); 
```

## 7.隐藏值–$('输入:隐藏')

要选择隐藏值

```java
 $('input:hidden'); 
```

为了得到隐藏的值

```java
 $('input:hidden').val(); 
```

要设置隐藏值

```java
 $('input:hidden').val("New Text"); 
```

## 8.选择(下拉框)–$('选择')

选择下拉框

```java
 $('select')
or
$('#dropdown-id') 
```

获取选定的下拉框值

```java
 $('#dropdown-id').val(); 
```

将下拉框值设置为“中国”

```java
 $("#dropdown-id").val("China"); 
```

[更多详情…](http://web.archive.org/web/20190227163628/http://www.mkyong.com/jquery/how-to-set-a-dropdown-box-value-in-jquery/)

## 9.提交按钮-$('输入:提交')

要选择提交按钮

```java
 $('input:submit'); 
```

## 10.重置按钮-$('输入:重置')

要选择复位按钮

```java
 $('input:reset'); 
```

## jQuery 表单选择器

```java
 <html>
<head>
<title>jQuery form selector example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div{
		padding:16px;
	}
</style>
</head>

<body>

<h1>jQuery form selector example</h1>

<h2>Message = <label id="msg"></label></h2>

<form name="testing" action="#">
  <div>
	TextBox : <input type="textbox" value="This is message from textbox" />
  </div>
  <div>
	Password : <input type="password" value="This is message from password" />
  </div>
  <div>
	TextArea : <textarea>This is message from textarea</textarea>
  </div>
  <div>
	Radio : <input name="sex" type="radio" value="Male" checked>Male</input>
	        <input name="sex" type="radio" value="Female">Female</input>
  </div>
  <div>
	CheckBox : <input type="checkbox" name="checkme">Check Me</input>
  </div>
  <div>
	File : <input type="file"></input>
  </div>
  <div>
	Hidden : <input type="hidden" value="This is message from hidden"/>
  </div>
  <div>
	Country : <select id="country">
			<option value="China">China</option>
			<option value="United State">United State</option>
		     </select>
  </div>
  <div>
	<input type="submit"></input> <input type="reset"></input>
  </div>	
</form>

<button id="text">input:text</button>
<button id="password">input:password</button>
<button id="textarea">textarea</button>
<button id="radio">input:radio</button>
<button id="checkbox">input:checkbox</button>
<button id="file">input:file</button>
<button id="hidden">input:hidden</button>
<button id="select">select</button>
<button id="submit">input:submit</button>
<button id="reset">input:reset</button>

<script type="text/javascript">
    $("#text, #password, #hidden, 
          #textarea, #file, #submit, #reset").click(function () {
	  var str = $(this).text();
	  $('input, textarea, select').css("background", "#ffffff");
	  $(str).css("background", "#ff0000");
	  $('#msg').html($(str).val())
    });

    $("#radio").click(function () {
	  $('input, textarea, select').css("background", "#ffffff");
	  $('#msg').html($('input:radio[name=sex]:checked').val());
    });

    $("#checkbox").click(function () {
	  var str = $(this).text();
	  $('input, textarea, select').css("background", "#ffffff");

	  if($('input:checkbox[name=checkme]').is(':checked')){
	  	$('#msg').html("Checkbox is checked");
	  }else{
	  	$('#msg').html("Checkbox is uncheck");
	  }	  
    });

    $("#select").click(function () {
	  $('input, textarea, select').css("background", "#ffffff");
	  $('#msg').html($('#country').val());
    });

</script>

</body>
</html> 
```

[http://web.archive.org/web/20190227163628if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-form-selector.html](http://web.archive.org/web/20190227163628if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-form-selector.html)

[Try Demo](http://web.archive.org/web/20190227163628/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-form-selector.html)[jquery](http://web.archive.org/web/20190227163628/http://www.mkyong.com/tag/jquery/) [jquery selector](http://web.archive.org/web/20190227163628/http://www.mkyong.com/tag/jquery-selector/)







