# Struts 2 拦截器堆栈示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-interceptor-stack-example/>

通常，同一组拦截器可以应用于不同的动作类，例如，

```java
 <package name="default" namespace="/" extends="struts-default">

    <action name="checkInAction" 
	class="com.mkyong.common.action.CheckInAction" >
	<interceptor-ref name="timer"/>
        <interceptor-ref name="logger"/>
	<interceptor-ref name="defaultStack" />
	<result name="success">pages/checkIn.jsp</result>
    </action>

    <action name="checkOutAction" 
	class="com.mkyong.common.action.CheckOutAction" >
	<interceptor-ref name="timer"/>
        <interceptor-ref name="logger"/>
	<interceptor-ref name="defaultStack" />
	<result name="success">pages/checkOut.jsp</result>
    </action>

</package> 
```

在上述情况下，它有许多重复的工作，根本不能重用。

幸运的是，Struts 2 附带了**拦截器栈**，允许开发人员将一组拦截器组合成一个名为“**栈名**的单元，动作可以通过“**栈名**引用它。

**Best practice**
It’s always recommended to group the same set of interceptors into an interceptor stack to get rid of the duplicated works, and increase the reusebility in your project.

```java
 <package name="default" namespace="/" extends="struts-default">

     <interceptors>
       	<interceptor-stack name="defaultStackWithLog">
             <interceptor-ref name="timer"/>
             <interceptor-ref name="logger"/>
	     <interceptor-ref name="defaultStack" />
        </interceptor-stack>
    </interceptors>

    <action name="checkInAction" 
	class="com.mkyong.common.action.CheckInAction" >
	<interceptor-ref name="defaultStackWithLog"/>
	<result name="success">pages/checkIn.jsp</result>
    </action>

    <action name="checkOutAction" 
	class="com.mkyong.common.action.CheckOutAction" >
	<interceptor-ref name="defaultStackWithLog"/>
	<result name="success">pages/checkOut.jsp</result>
    </action>

</package> 
```

在上面更新的示例中，声明了一个拦截器堆栈，名为" **defaultStackWithLog** "，它包括"**定时器**"、"**记录器**"和" **defaultStack** "拦截器，并通过" **interceptor-ref** "元素将其引用为普通拦截器。

## 参考

1.  [Struts 2 拦截器文档](http://web.archive.org/web/20190214233721/http://struts.apache.org/2.1.8/docs/interceptors.html)

[interceptor](http://web.archive.org/web/20190214233721/http://www.mkyong.com/tag/interceptor/) [struts2](http://web.archive.org/web/20190214233721/http://www.mkyong.com/tag/struts2/)![](img/7b5dc0617ddade9fb72642caf423f6e0.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214233721/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="6267">







