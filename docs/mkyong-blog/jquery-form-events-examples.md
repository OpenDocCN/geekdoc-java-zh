# jQuery 表单事件示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/jquery-form-events-examples/>

jQuery 提供了五个常见的表单事件来处理表单元素的动作。

## 1.焦点()

当元素处于焦点时激发。

```java
 $("input,select,textarea").focus(function () {
	//do something
}); 
```

 ## 2.模糊()

当元素失去焦点时激发。

```java
 $("input,select,textarea").blur(function () {
	//do something
}); 
```

 ## 3.更改()

当元素值改变时触发，例如更新复选框，单选按钮或文本框值。

```java
 $("input,select,textarea").change(function () {
	//do something
}); 
```

## 4.选择()

突出显示元素内的文本时触发，仅限于 textbox 或 textarea。

```java
 $("input,textarea").focus(function () {
	//do something
}); 
```

## 5.提交()

试图提交表单时触发，仅绑定到表单元素。

```java
 $("form").focus(function () {
	//do something
}); 
```

## 你自己试试

```java
 <html>
<head>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div{
		padding:16px;
	}
	.focus, .blur, .change, .select{
		color:white;
		border:1px solid red;
		background-color:blue;
		padding:8px;
		margin:8px;
	}
</style>
</head>

<body>

<h1>jQuery form events - focus(), change(), blur(), select(), submit() example</h1>

<form name="testing" action="#">
<div>
   TextBox : <input type="textbox" size="50"></input>
</div>
<div>
   <label style="float:left">TextArea : </label> 
   <textarea cols="30" rows="5"></textarea>
</div>
<div>
   Radio : <input name="sex" type="radio" value="Male" checked>Male</input>
	     <input name="sex" type="radio" value="Female">Female</input>
</div>
<div>
   CheckBox : <input type="checkbox" name="checkme">Check Me</input>
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

<script type="text/javascript">

    $("input,select,textarea").focus(function () {

	  $(this).after("<span class='focus'> focus() triggered! </span>");
	  $("span").filter('.focus').fadeOut(4000);

    });

    $("input,select,textarea").blur(function () {

	  $(this).after("<span class='blur'> blur() triggered! </span>");
	  $("span").filter('.blur').fadeOut(4000);

    });

    $("input,select,textarea").change(function () {

	  $(this).after("<span class='change'> change() triggered! </span>");
	  $("span").filter('.change').fadeOut(4000);

    });

    $("input,textarea").select(function () {

	  $(this).after("<span class='select'> select() triggered! </span>");
	  $("span").filter('.select').fadeOut(4000);

    });

    $("form").submit(function () {

	 	alert('Form submitted!');

    });	
</script>

</body>
</html> 
```

[http://web.archive.org/web/20190309091101if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-form-events.html](http://web.archive.org/web/20190309091101if_/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-form-events.html)

[Try Demo](http://web.archive.org/web/20190309091101/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-form-events.html)[form event](http://web.archive.org/web/20190309091101/http://www.mkyong.com/tag/form-event/) [jquery](http://web.archive.org/web/20190309091101/http://www.mkyong.com/tag/jquery/)







