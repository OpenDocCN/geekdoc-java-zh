# jQuery–如何禁用点击后的提交按钮

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-disable-submit-button-after-clicked-with-jquery/>

通常，用户喜欢在提交按钮上按几次以确保按钮被点击，这导致了双重表单提交问题。常见的解决方案是在用户点击提交按钮后禁用它。

## 1.启用/禁用提交按钮

1.1 要禁用一个提交按钮，你只需要给提交按钮添加一个`disabled`属性。

```java
 $("#btnSubmit").attr("disabled", true); 
```

1.2 要启用禁用的按钮，请将`disabled`属性设置为 false，或者移除`disabled`属性。

```java
 $('#btnSubmit').attr("disabled", false);	
or
$('#btnSubmit').removeAttr("disabled"); 
```

## 2.jQuery 完整示例

```java
 <!DOCTYPE html>
<html lang="en">

<body>
<h1>jQuery - How to disabled submit button after clicked</h1>

<form id="formABC" action="#" method="POST">
    <input type="submit" id="btnSubmit" value="Submit"></input>
</form>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>

<input type="button" value="i am normal abc" id="btnTest"></input>

<script>
    $(document).ready(function () {

        $("#formABC").submit(function (e) {

            //stop submitting the form to see the disabled button effect
            e.preventDefault();

            //disable the submit button
            $("#btnSubmit").attr("disabled", true);

            //disable a normal button
            $("#btnTest").attr("disabled", true);

            return true;

        });
    });
</script>

</body>
</html> 
```

## 参考

1.  [jQuery 提交 API](http://web.archive.org/web/20221023054402/https://api.jquery.com/submit/)

<input type="hidden" id="mkyong-current-postId" value="5198">