# 如何用 jQuery 动态添加/删除文本框

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-add-remove-textbox-dynamically-with-jquery/>

在 jQuery 中，动态添加或删除文本框非常容易。这个想法很简单，只是结合使用 **'counter** '变量，jQuery **createElement()** ， **html()** 和 **remove()** 方法。请参见下面的示例:

## jQuery 动态文本框示例

```java
 <html>
<head>
<title>jQuery add / remove textbox example</title>

<script type="text/javascript" src="jquery-1.3.2.min.js"></script>

<style type="text/css">
	div{
		padding:8px;
	}
</style>

</head>

<body>

<h1>jQuery add / remove textbox example</h1>

<script type="text/javascript">

$(document).ready(function(){

    var counter = 2;

    $("#addButton").click(function () {

	if(counter>10){
            alert("Only 10 textboxes allow");
            return false;
	}   

	var newTextBoxDiv = $(document.createElement('div'))
	     .attr("id", 'TextBoxDiv' + counter);

	newTextBoxDiv.after().html('<label>Textbox #'+ counter + ' : </label>' +
	      '<input type="text" name="textbox' + counter + 
	      '" id="textbox' + counter + '" value="" >');

	newTextBoxDiv.appendTo("#TextBoxesGroup");

	counter++;
     });

     $("#removeButton").click(function () {
	if(counter==1){
          alert("No more textbox to remove");
          return false;
       }   

	counter--;

        $("#TextBoxDiv" + counter).remove();

     });

     $("#getButtonValue").click(function () {

	var msg = '';
	for(i=1; i<counter; i++){
   	  msg += "\n Textbox #" + i + " : " + $('#textbox' + i).val();
	}
    	  alert(msg);
     });
  });
</script>
</head><body>

<div id='TextBoxesGroup'>
	<div id="TextBoxDiv1">
		<label>Textbox #1 : </label><input type='textbox' id='textbox1' >
	</div>
</div>
<input type='button' value='Add Button' id='addButton'>
<input type='button' value='Remove Button' id='removeButton'>
<input type='button' value='Get TextBox Value' id='getButtonValue'>

</body>
</html> 
```

[http://web.archive.org/web/20221225130025if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-add-remove-textbox.html](http://web.archive.org/web/20221225130025if_/https://www.mkyong.com/wp-content/uploads/jQuery/jQuery-add-remove-textbox.html)

[Try Demo](http://web.archive.org/web/20221225130025/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-add-remove-textbox.html)<input type="hidden" id="mkyong-current-postId" value="5031">