# struts 2–为操作类配置静态参数

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/struts-2-configure-static-parameter-for-action-class/>

Download It – [Struts-Action-Static-ParamExample.zip](http://web.archive.org/web/20190601041554/http://www.mkyong.com/wp-content/uploads/2010/06/Struts-Action-Static-ParamExample.zip)

在某些情况下，您可能需要为动作类分配一些预定义的或静态的参数值。

## 定义了动作的静态参数

在 Struts 2 中，可以在 **struts.xml** 文件中进行配置，通过 **< param >** 标签，例如

**struts.xml**

```java
 <struts>

   <constant name="struts.custom.i18n.resources" value="global" />
   <constant name="struts.devMode" value="true" />

   <package name="default" namespace="/" extends="struts-default">
	<action name="locale" class="com.mkyong.common.action.LocaleAction">
		<result name="SUCCESS">pages/welcome.jsp</result>
		<param name="EnglishParam">English</param>
    	        <param name="ChineseParam">Chinese</param>
     	        <param name="FranceParam">France</param>
	</action>
   </package>	
</struts> 
```

它将三个预定义参数值分配给一个 **LocaleAction** 操作类。

 ## 从操作中获取静态参数

为了从 **struts.xml** 中获取静态参数值，Action 类必须实现**参数化**接口。它可以通过**地图属性**或 **JavaBean** 属性来访问。

The Action’s static parameters is control by the **staticParams Interceptor**, which is included in the default stack “struts-default.xml”. ## 1.地图属性

在动作类初始化期间，staticParams 拦截器将通过 **setParams()** 方法将预定义参数值的引用传递给动作类。

```java
 //...
import com.opensymphony.xwork2.config.entities.Parameterizable;

public class LocaleAction implements Parameterizable{

	Map<String, String> params;
	//...
	public void setParams(Map<String, String> params) {
		this.params = params;
	}
} 
```

## 2.JavaBean 属性

在 Action 类初始化期间，如果您正确地创建了 getter 和 setter 方法，staticParams 拦截器会将预定义的参数值设置为对应于“param”元素的每个 JavaBean 属性。

```java
 //...
import com.opensymphony.xwork2.config.entities.Parameterizable;

public class LocaleAction implements Parameterizable{

	String englishParam;
	String chineseParam;
	String franceParam;

	public String getEnglishParam() {
		return englishParam;
	}

	public void setEnglishParam(String englishParam) {
		this.englishParam = englishParam;
	}

	public String getChineseParam() {
		return chineseParam;
	}

	public void setChineseParam(String chineseParam) {
		this.chineseParam = chineseParam;
	}

	public String getFranceParam() {
		return franceParam;
	}

	public void setFranceParam(String franceParam) {
		this.franceParam = franceParam;
	}
    //...
} 
```

[struts2](http://web.archive.org/web/20190601041554/https://www.mkyong.com/tag/struts2/)







