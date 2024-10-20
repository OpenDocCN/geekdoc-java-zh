# JSF 新协议国际化范例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/jsf2/jsf-2-internationalization-example/>

在 JSF 应用程序中，您可以像这样以编程方式更改应用程序区域设置:

```java
 //this example change locale to france
FacesContext.getCurrentInstance().getViewRoot().setLocale(new Locale('fr'); 
```

这使得 JSF 很容易支持国际化或多语言。

## 完整的 JSF 国际化范例

在本教程中，我们将向您展示一个 JSF 2.0 web 应用程序，它显示一个欢迎页面，从属性文件中检索欢迎消息，并根据所选语言动态更改欢迎消息。

freestar.config.enabled_slots.push({ placementName: "mkyong_incontent_1", slotId: "mkyong_incontent_1" });

## 1.项目文件夹

本例的目录结构。



![jsf2-internationalization-folder](img/ddb2a22e1c8e57cd2fc646428c00b050.png "jsf2-internationalization-folder")

## 2.属性文件

这里有两个属性文件来存储英文和中文信息。

*welcome.properties*

```java
 welcome.jsf = Happy learning JSF 2.0 
```

*welcome_zh_CN.properties*

```java
 welcome.jsf = \u5feb\u4e50\u5b66\u4e60 JSF 2.0 
```

**Note**
For UTF-8 or non-English characters, for example Chinese , you should encode it with [native2ascii](http://web.archive.org/web/20210224055858/http://www.mkyong.com/java/java-convert-chinese-character-to-unicode-with-native2ascii/) tool.

## 3.faces-config.xml

包括到您的 JSF 应用程序中，并声明“en”作为您的默认应用程序区域设置。

*faces-config.xml*

```java
 <?xml version="1.0" encoding="UTF-8"?>
<faces-config

    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
    http://java.sun.com/xml/ns/javaee/web-facesconfig_2_0.xsd"
    version="2.0">
     <application>
     	   <locale-config>
     	        <default-locale>en</default-locale>
     	   </locale-config>
	   <resource-bundle>
		<base-name>com.mkyong.welcome</base-name>
		<var>msg</var>
	   </resource-bundle>
     </application>
</faces-config> 
```

## 4.受管 Bean

一个受管 bean，它提供语言选择列表和一个值更改事件侦听器，以编程方式更改区域设置。

LanguageBean。java

```java
 package com.mkyong;

import java.io.Serializable;
import java.util.LinkedHashMap;
import java.util.Locale;
import java.util.Map;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.faces.context.FacesContext;
import javax.faces.event.ValueChangeEvent;

@ManagedBean(name="language")
@SessionScoped
public class LanguageBean implements Serializable{

	private static final long serialVersionUID = 1L;

	private String localeCode;

	private static Map<String,Object> countries;
	static{
		countries = new LinkedHashMap<String,Object>();
		countries.put("English", Locale.ENGLISH); //label, value
		countries.put("Chinese", Locale.SIMPLIFIED_CHINESE);
	}

	public Map<String, Object> getCountriesInMap() {
		return countries;
	}

	public String getLocaleCode() {
		return localeCode;
	}

	public void setLocaleCode(String localeCode) {
		this.localeCode = localeCode;
	}

	//value change event listener
	public void countryLocaleCodeChanged(ValueChangeEvent e){

		String newLocaleValue = e.getNewValue().toString();

		//loop country map to compare the locale code
                for (Map.Entry<String, Object> entry : countries.entrySet()) {

        	   if(entry.getValue().toString().equals(newLocaleValue)){

        		FacesContext.getCurrentInstance()
        			.getViewRoot().setLocale((Locale)entry.getValue());

        	  }
               }
	}

} 
```

## 5.JSF·佩奇

一个 JSF 页面，显示来自属性文件的欢迎消息，并将值更改事件侦听器附加到下拉框。

```java
 <?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html    
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      >

    <h:body>

    	<h1>JSF 2 internationalization example</h1>

     <h:form>

	<h2>
		<h:outputText value="#{msg['welcome.jsf']}" />
	</h2>

	<h:panelGrid columns="2">

		Language : 
		<h:selectOneMenu value="#{language.localeCode}" onchange="submit()"
			valueChangeListener="#{language.countryLocaleCodeChanged}">
   			<f:selectItems value="#{language.countriesInMap}" /> 
   		</h:selectOneMenu>

	</h:panelGrid>

      </h:form>

    </h:body>
</html> 
```

## 6.演示

URL:*http://localhost:8080/Java server faces/faces/default . XHTML*

默认情况下，显示区域英语。



![jsf2-internationalization-example-1](img/08714e2efbde577a135be15a88e86333.png "jsf2-internationalization-example-1")

如果用户更改下拉框语言，它将触发一个值更改事件监听器，并相应地更改应用程序的区域设置。



![jsf2-internationalization-example-2](img/67e6fc31c9ea86ee49f734f29fcfcfde.png "jsf2-internationalization-example-2")

## 下载源代码

Download It – [JSF-2-Internationalization-Example.zip](http://web.archive.org/web/20210224055858/http://www.mkyong.com/wp-content/uploads/2010/11/JSF-2-Internationalization-Example.zip) (11KB)

## 参考

1.  [JSF 2.0 中的资源包](http://web.archive.org/web/20210224055858/http://www.mkyong.com/jsf2/jsf-2-0-and-resource-bundles-example/)
2.  [创建语言环境(Oracle 教程)](http://web.archive.org/web/20210224055858/https://download.oracle.com/javase/tutorial/i18n/locale/create.html)
3.  [Spring MVC 国际化示例](http://web.archive.org/web/20210224055858/http://www.mkyong.com/spring-mvc/spring-mvc-internationalization-example/)
4.  [W3C 字符集](http://web.archive.org/web/20210224055858/https://www.w3.org/International/O-HTTP-charset)

Tags : [jsf2](http://web.archive.org/web/20210224055858/https://mkyong.com/tag/jsf2/) [multiple languages](http://web.archive.org/web/20210224055858/https://mkyong.com/tag/multiple-languages/)freestar.config.enabled_slots.push({ placementName: "mkyong_leaderboard_btf", slotId: "mkyong_leaderboard_btf" });<input type="hidden" id="mkyong-current-postId" value="7608">