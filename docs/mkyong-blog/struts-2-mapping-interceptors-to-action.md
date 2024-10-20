# Struts 2 将拦截器映射到动作

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-mapping-interceptors-to-action/>

Struts 2 开发者用来声明动作属于一个扩展了" **struts-default** "的包，其中包含了默认的拦截器集合。

```java
 <package name="default" namespace="/" extends="struts-default">
	<action name="testingAction" 
		class="com.mkyong.common.action.TestingAction" >
		<result name="success">pages/result.jsp</result>
	</action>
</package> 
```

默认的拦截器集合在 **struts-default.xml** 文件中被分组为“ **defaultStack** ，该文件位于 **struts2-core.jar** 文件中。“ **defaultStack** ”提供了 Struts 2 的所有核心功能，满足了大多数应用程序的需求。

Try study the **struts-default.xml** file, it’s always the best interceptors reference.

## 将拦截器映射到操作

要将其他拦截器映射到动作，可以使用" **interceptor-ref** "元素。

```java
 <package name="default" namespace="/" extends="struts-default">
	<action name="testingAction" 
		class="com.mkyong.common.action.TestingAction" >
		<interceptor-ref name="timer"/>
		<interceptor-ref name="logger"/>
		<result name="success">pages/result.jsp</result>
	</action>
</package> 
```

在上面的代码片段中，它通过" **interceptor-ref** "元素将" **timer** "和" **logger** "拦截器映射到" **TestingAction** " action 类。

The interceptors will fire in the order they’re declared.

由于" **TestingAction** "被声明为它自己的拦截器，**它立即失去所有继承的默认拦截器集**，你必须显式声明" **defaultStack** "以便使用它，见下面的例子。

```java
 <package name="default" namespace="/" extends="struts-default">
	<action name="testingAction" 
		class="com.mkyong.common.action.TestingAction" >
		<interceptor-ref name="timer"/>
		<interceptor-ref name="logger"/>
		<interceptor-ref name="defaultStack"/>
		<result name="success">pages/result.jsp</result>
	</action>
</package> 
```

 ## 参考

1.  [Struts 2 拦截器文档](http://web.archive.org/web/20190224162049/http://struts.apache.org/2.1.8/docs/interceptors.html)

[interceptor](http://web.archive.org/web/20190224162049/http://www.mkyong.com/tag/interceptor/) [struts2](http://web.archive.org/web/20190224162049/http://www.mkyong.com/tag/struts2/)![](img/2e1bf2daefe6b8871b8d164006720c91.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224162049/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6251">







