# JSF 2.0 : <ajax>包含一个未知的 id</ajax>

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-0-f-ajax-contains-an-unknown-id/>

## 问题

一个支持 Ajax 的 JSF 按钮…

```java
 <h:outputText id="output" value="#{helloBean.sayWelcome}" />
<h:form>    	
   <h:inputText id="name" value="#{helloBean.name}"></h:inputText>
   <h:commandButton value="Welcome Me">
   	 <f:ajax execute="name" render="output" />
   </h:commandButton>
</h:form> 
```

当显示此页面时，它会提示以下错误消息

```java
 javax.faces.FacesException: <f:ajax> contains an unknown id 'output'
                             - cannot locate it in the context of the component j_idt8 
```

显然，‘**输出**的 id 没有找到，但是已经在**<h:output text id = " output "/>**中明确声明了？

 ## 解决办法

在 JSF 2.0 中， **< f:ajax >** 标签要求在同一个表单级别内进行“渲染”输出。**<h:output text id = " output "/>**标签应该在表单内移动。

```java
 <h:form>    	
   <h:outputText id="output" value="#{helloBean.sayWelcome}" />
   <h:inputText id="name" value="#{helloBean.name}"></h:inputText>
   <h:commandButton value="Welcome Me">
   	 <f:ajax execute="name" render="output" />
   </h:commandButton>
</h:form> 
```

 ## 参考

1.  [JSF 2.0 + Ajax hello world 示例](http://web.archive.org/web/20190113095434/http://www.mkyong.com/jsf2/jsf-2-0-ajax-hello-world-example/)

[ajax](http://web.archive.org/web/20190113095434/http://www.mkyong.com/tag/ajax/) [jsf2](http://web.archive.org/web/20190113095434/http://www.mkyong.com/tag/jsf2/)







