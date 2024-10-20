# 如何用 jQuery 创建表格斑马条纹效果

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jquery/how-to-create-a-table-zebra-stripes-effect-jquery/>

下面是一个简单的例子，展示了如何用 jQuery 创建一个表格斑马条纹效果。

```java
 <html>
<head>
<title>jQuery Zebra Stripes</title>
</head>
<script type="text/javascript" src="jquery-1.2.6.min.js"></script>

	<script type="text/javascript">
      $(function() {
        $("table tr:nth-child(even)").addClass("striped");
      });
    </script>

    <style type="text/css">
      body,td {
        font-size: 10pt;
      }
      table {
        background-color: black;
        border: 1px black solid;
        border-collapse: collapse;
      }
      th {
        border: 1px outset silver;
        background-color: maroon;
        color: white;
      }
      tr {
        background-color: white;
        margin: 1px;
      }
      tr.striped {
        background-color: coral;
      }
      td {
        padding: 1px 8px;
      }
    </style>

<body>
    <table>
      <tr>
        <th>ID</th>
        <th>Fruit</th>
        <th>Price</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Apple</td>
        <td>0.60</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Orange</td>
        <td>0.50</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Banana</td>
        <td>0.10</td>
      </tr>
      <tr>
        <td>4</td>
        <td>strawberry</td>
        <td>0.05</td>
      </tr>
      <tr>
        <td>5</td>
        <td>carrot</td>
        <td>0.10</td>
      </tr>
    </table>
</body>
</html> 
```

![jquery-zebra-stripes](img/b4f1e720cb5e610263daeb2222c79890.png "jquery-zebra-stripes")

在 jQuery 中，表格斑马条纹效果是通过一条语句实现的。

```java
 $(function() {
        $("table tr:nth-child(even)").addClass("striped");
}); 
```

*P.S* 第 n 个孩子(偶数)”)。addClass("striped") =每个偶数行动态添加" striped" CSS 类。

[Try Demo](http://web.archive.org/web/20220618072947/http://www.mkyong.com/wp-content/uploads/jQuery/jQuery-zebra-stripes-effect.html)<input type="hidden" id="mkyong-current-postId" value="359">