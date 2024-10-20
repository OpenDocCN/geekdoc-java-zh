# 如何在 JSF 2.0 中使用注释

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/how-to-use-comments-in-jsf-2-0/>

## 问题

在 JSF 2.0 中，注释掉一个 JSF 标签，如下所示

*JSF…*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      >
     <h:body>

     <!-- 
    	<h:commandButton type="button" 
    		value="#{msg.buttonLabel}" />
      -->

    </h:body>
</html> 
```

但是 JSF 仍然处理值表达式并将结果输出到生成的 HTML 页面。假设 **#{msg.buttonLabel}** 正在返回“提交”消息。

*生成的 HTML 页面…*

```java
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "
http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html >
   <body> 
     <!-- 
    	<h:commandButton type="button" 
    		value="Submit" />
      -->
   </body> 
</html> 
```

有没有办法完全注释掉 JSF 标签？没有对值表达式进行处理或者出现在最终生成的 HTML 页面中？

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 解决办法

有两种方法可以注释掉 JSF 标记:

## 1.facelets。跳过评论

在 web.xml 中，设置“ **facelets。SKIP_COMMENTS** 参数为 **true** 。

```java
 <context-param>
    <param-name>facelets.SKIP_COMMENTS</param-name>
    <param-value>true</param-value>
</context-param> 
```

现在，JSF 删除页面中包含在 **<中的任何内容！–—>**。

*JSF…*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      >
     <h:body>

      <!-- 
    	<h:commandButton type="button" 
    		value="#{msg.buttonLabel}" />
       -->

     </h:body>
</html> 
```

*生成的 HTML 页面…*

```java
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "
http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html >
	<body> 

	</body> 
</html> 
```

## 2.用户界面:删除

或者，您可以使用“ **ui:remove** ”标签来定义您想要删除的内容。举个例子，

*JSF*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      >
      <h:body>

        <ui:remove>
    	  <h:commandButton type="button" 
    		value="#{msg.buttonLabel}" />
        </ui:remove>

      </h:body>
</html> 
```

*生成的 HTML 页面…*

```java
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "
http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
<html >
	<body> 

	</body> 
</html> 
```

## 下载源代码

Download It – [JSF-2-Remove-Tag-Example.zip](http://web.archive.org/web/20210211172639/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-Remove-Tag-Example.zip) (10KB)

#### 参考

1.  [JSF " ui:remove " JavaDoc](http://web.archive.org/web/20210211172639/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/ui/remove.html)

标签:[JSF 2](http://web.archive.org/web/20210211172639/https://mkyong.com/tag/jsf2/)freestar . config . enabled _ slots . push({ placement name:" mkyong _ leader board _ btf "，slotId:" mkyong _ leader board _ btf " })；【T12<input type="hidden" id="mkyong-current-postId" value="7353">