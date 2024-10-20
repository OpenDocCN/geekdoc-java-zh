# 如何在 jQuery 中设置下拉框值

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-set-a-dropdown-box-value-in-jquery/>

一个简单的选择/下拉框，id=“国家”。

```java
 <select id="country">
   <option value="None">-- Select --</option>
   <option value="China">China</option>
   <option value="United State">United State</option>
   <option value="Malaysia">Malaysia</option>
</select> 
```

1.显示选定的下拉框值。

```java
 $('#country').val(); 
```

2.将下拉框值设置为“中国”。

```java
 $("#country").val("China"); 
```

3.将下拉框值设置为“美国”。

```java
 $("#country").val("United State"); 
```

4.将下拉框值设置为“马来西亚”。

```java
 $("#country").val("Malaysia"); 
```

5.禁用下拉框中的“美国”选项。

```java
 $("#country option[value='United State']").attr("disabled", true); 
```

6.启用下拉框中的“美国”选项。

```java
 $("#country option[value='United State']").attr("disabled", false); 
```

## jQuery 选择/下拉框示例

```java
 <html>
<head>
<title>jQuery select / dropdown box example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

</head>

<body>

<h1>jQuery select / dropdown box example</h1>

<script type="text/javascript">

  $(document).ready(function(){

    $("#isSelect").click(function () {

	alert($('#country').val());

    });

    $("#selectChina").click(function () {

	$("#country").val("China");

    });

    $("#selectUS").click(function () {

	$("#country").val("United State");

    });

    $("#selectMalaysia").click(function () {

	$("#country").val("Malaysia");

    });

    $("#disableUS").click(function () {

	$("#country option[value='United State']").attr("disabled", true);

    });

    $("#enableUS").click(function () {

	$("#country option[value='United State']").attr("disabled", false);

    });

  });
</script>
</head><body>

<select id="country">
<option value="None">-- Select --</option>
<option value="China">China</option>
<option value="United State">United State</option>
<option value="Malaysia">Malaysia</option>
</select>

<br/>
<br/>
<br/>

<input type='button' value='Display Selected' id='isSelect'>
<input type='button' value='Select China' id='selectChina'>
<input type='button' value='Select US' id='selectUS'>
<input type='button' value='Select Malaysia' id='selectMalaysia'>
<input type='button' value='Disable US' id='disableUS'>
<input type='button' value='Enable US' id='enableUS'>
</body>
</html> 
```

[http://web.archive.org/web/20221006023910if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-dropdown-box.html](http://web.archive.org/web/20221006023910if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-dropdown-box.html)

[Try Demo](http://web.archive.org/web/20221006023910/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-select-dropdown-box.html)<input type="hidden" id="mkyong-current-postId" value="5055">