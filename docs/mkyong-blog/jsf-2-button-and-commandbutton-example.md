# JSF 2 按钮和命令按钮示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-button-and-commandbutton-example/>

在 JSF 2.0 中， **< h:button / >** 和 **< h:commandButton / >** 标签都是用来渲染按钮类型的 HTML 输入元素，用不同的机制来处理导航。

## 1.JSF h:命令按钮示例

“ **h:commandButton** 标签是 JSF 1.x 以后发布的，可以声明 bean，它在“ **action** 属性中返回导航结果。如果浏览器禁用了 JavaScript，导航仍然可以工作，因为导航是通过表单 post 处理的。

1.提交按钮

```java
 //JSF
<h:commandButton value="submit" type="submit" action="#{user.goLoginPage}" />

//HTML output
<input type="submit" name="xxx" value="submit" /> 
```

2.复原按钮

```java
 //JSF
<h:commandButton value="reset" type="reset" />

//HTML output
<input type="reset" name="xxx" value="reset" /> 
```

3.正常按钮

```java
 //JSF
<h:commandButton value="button" type="button" />

//HTML output
<input type="button" name="xxx" value="button" /> 
```

4.带有 onclick 事件的普通按钮

```java
 //JSF
<h:commandButton value="Click Me" type="button" onclick="alert('h:commandButton');" />	

//HTML output
<input type="button" name="xxx" value="Click Me" onclick="alert('h:commandButton');" /> 
```

## 2.JSF h:按钮示例

“ **h:button** 是 JSF 2.0 中新增的标签，可以直接在“ **outcome** 属性中声明导航结果，不需要像上面的“h:commandButton”那样调用 bean 返回结果。但是，如果浏览器禁用了 JavaScript，导航就会失败，因为“ **h:button** ”标签会生成一个“onclick”事件来通过“window.location.href”处理导航。参见示例:

1.没有结果的正常按钮

```java
 //JSF
<h:button value="buton" />			

//HTML output
<input type="button" 
   onclick="window.location.href='/JavaServerFaces/faces/currentpage.xhtml; return false;" 
   value="buton" /> 
```

如果省略了 outcome 属性，当前页面 URL 将被视为结果。

2.有结果的正常按钮

```java
 //JSF
<h:button value="buton" outcome="login" />			

//HTML output
<input type="button" 
   onclick="window.location.href='/JavaServerFaces/faces/login.xhtml; return false;" 
   value="buton" /> 
```

3.带有 JavaScript 的普通按钮。

```java
 //JSF
<h:button value="Click Me" onclick="alert('h:button');" />

//HTML output
<input type="button" 
   onclick="alert('h:button');window.location.href='/JavaServerFaces/faces/page.xhtml;return false;" 
   value="Click Me" /> 
```

**My thought…**
No really sure why JSF 2.0 released this “**h:button**” tag, the JavaScript redirection is not practical, especially in JavaScript disabled browser. The best is integrate the “**outcome**” attribute into the “**h:commandButton**” tag, hope it can be done in future release.

## 下载源代码

Download It – [JSF-2-Button-CommandButton-Example.zip](http://web.archive.org/web/20210108031203/http://www.mkyong.com/wp-content/uploads/2010/10/JSF-2-Button-CommandButton-Example.zip) (10KB)

#### 参考

1.  [JSF<h:button/>JavaDoc](http://web.archive.org/web/20210108031203/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/h/button.html)
2.  [JSF < h:命令按钮/ > JavaDoc](http://web.archive.org/web/20210108031203/https://javaserverfaces.dev.java.net/nonav/docs/2.0/pdldocs/facelets/h/commandButton.html)

标签:[按钮](http://web.archive.org/web/20210108031203/https://mkyong.com/tag/button/)[JSF 2](http://web.archive.org/web/20210108031203/https://mkyong.com/tag/jsf2/)<input type="hidden" id="mkyong-current-postId" value="7228">

### 相关文章

*   [ JSF 2.0 tutorial
*   [JSF 2 预渲染阀 103](/web/20210108031203/https://www.mkyong.com/jsf2/jsf-2-prerenderviewevent-example/)
*   [ JSF 2.0 【T1] Multi-component verifier

*   [JSF 2 列表框 103](/web/20210108031203/https://www.mkyong.com/jsf2/jsf-2-listbox-example/)
*   [JSF 新协议数据表示例](/web/20210108031203/https://www.mkyong.com/jsf2/jsf-2-datatable-example/)
*   [Example of JSF2 radio button](/web/20210108031203/https://www.mkyong.com/jsf2/jsf-2-radio-buttons-example/)
*   [Warning: JSF1063: Warning! Set unseriable](/web/20210108031203/https://www.mkyong.com/jsf2/warning-jsf1063-warning-setting-non-serializable-attribute-value-into-httpsession/)
*   [JSF2 drop-down box example](/web/20210108031203/https://www.mkyong.com/jsf2/jsf-2-dropdown-box-example/)