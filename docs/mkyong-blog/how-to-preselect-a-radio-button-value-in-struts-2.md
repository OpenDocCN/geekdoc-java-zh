# 如何在 Struts 2 中预选单选按钮值

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/struts2/how-to-preselect-a-radio-button-value-in-struts-2/>

Download It – [Struts2-Radio-Button-Example.zip](http://web.archive.org/web/20190515054916/http://www.mkyong.com/wp-content/uploads/2010/06/Struts2-Radio-Button-Example.zip)

这里有几个 Struts 2 的例子来演示如何为一个单选按钮预先选择一个默认值，这个默认值是通过列表、OGNL 和对象生成的。

## 1.目录

在 Java 代码中，创建一个列表来返回单选按钮的值。

```java
 //...
	List<String> genders = new ArrayList<String>();
		genders.add("male");
		genders.add("female");
		genders.add("unknown");
	//...
	public List<String> getGenders() {
		return genders;
	}
	public String getDefaultGenderValue(){
		return "unknown";
	}
	//... 
```

在结果页面的<radio>标签中， **list="genders"** 将调用 **getGenders()** 方法来返回单选按钮的键和值的列表。并且**value = " defaultGenderValue "**会调用 **getDefaultGenderValue()** 方法来**预选“未知”值作为单选按钮的默认值**。</radio>

```java
 <s:radio label="Gender" name="yourGender" list="genders" value="defaultGenderValue" /> 
```

它将生成以下 HTML 代码…

```java
 <input type="radio" name="yourGender" id="resultAction_yourGendermale" value="male"/>
<label for="resultAction_yourGendermale">male</label> 

<input type="radio" name="yourGender" id="resultAction_yourGenderfemale" value="female"/>
<label for="resultAction_yourGenderfemale">female</label> 

<input type="radio" name="yourGender" id="resultAction_yourGenderunknown" 
   checked="checked" value="unknown"/>
<label for="resultAction_yourGenderunknown">unknown</label> 
```

 ## 2.OGNL

在结果页面中，通过 OGNL 表达式创建一个单选按钮，**预选“2”作为默认值**。

```java
 <s:radio label="Answer" name="yourAnswer" list="#{'1':'Yes','2':'No'}" value="2" /> 
```

它将生成以下 HTML 代码…

```java
 <input type="radio" name="yourAnswer" id="resultAction_yourAnswer1" value="1"/>
<label for="resultAction_yourAnswer1">Yes</label> 

<input type="radio" name="yourAnswer" id="resultAction_yourAnswer2" 
checked="checked" value="2"/>
<label for="resultAction_yourAnswer2">No</label> 
```

 ## 3.目标

在 Java 代码中，使用 languageCode 和 languageDisplay 属性创建一个语言对象。

```java
 //...
        public class Language{

	       private String languageCode;
	       private String languageDisplay;
	       //getter and setter methods
        } 
```

```java
 //...
	List<Language> languages = new ArrayList<Language>();
		languages.add( new Language("EN", "English") );
		languages.add( new Language("FR", "France") );
		languages.add( new Language("CN_ZH", "Chinese") );
		languages.add( new Language("DE", "German") );
	//...
	public List<Language> getLanguages() {
		return languages;
	}
	public String getDefaultLanguageValue(){
		return "CN_ZH";
	}
	//... 
```

在结果页面的<radio>标签中， **list="languages"** 将调用 **getLanguages()** 方法来返回单选按钮的键和值的列表。 **listKey="languageCode"** 表示该语言的 languageCode 属性为单选按钮的按键；**list value = " languageDisplay "**将语言的 Language display 属性指定为单选按钮的值。最后一个**value = " defaultLanguageValue "**会调用**getDefaultLanguageValue()**方法来**预选“CN_ZH”作为单选按钮的默认值**。</radio>

```java
 <s:radio label="Gender" name="yourLanguage" list="languages" 
 listKey="languageCode" listValue="languageDisplay" value="defaultLanguageValue" /> 
```

它将生成以下 HTML 代码…

```java
 <input type="radio" name="yourLanguage" id="resultAction_yourLanguageEN" value="EN"/>
<label for="resultAction_yourLanguageEN">English</label> 

<input type="radio" name="yourLanguage" id="resultAction_yourLanguageFR" value="FR"/>
<label for="resultAction_yourLanguageFR">France</label> 

<input type="radio" name="yourLanguage" id="resultAction_yourLanguageCN_ZH" 
checked="checked" value="CN_ZH"/>
<label for="resultAction_yourLanguageCN_ZH">Chinese</label> 

<input type="radio" name="yourLanguage" id="resultAction_yourLanguageDE" value="DE"/>
<label for="resultAction_yourLanguageDE">German</label> 
```

## 参考

1.  [http://struts.apache.org/2.0.11.2/docs/radio.html](http://web.archive.org/web/20190515054916/http://struts.apache.org/2.0.11.2/docs/radio.html)
2.  [http://struts . Apache . org/2 . 1 . 8 . 1/docs/struts-2-form-tags . html](http://web.archive.org/web/20190515054916/http://struts.apache.org/2.1.8.1/docs/struts-2-form-tags.html)
3.  [在 Struts 2 中创建一个单选按钮](http://web.archive.org/web/20190515054916/http://www.mkyong.com/struts2/struts-2-sradio-radio-button-example/)

[radio button](http://web.archive.org/web/20190515054916/https://www.mkyong.com/tag/radio-button/) [struts2](http://web.archive.org/web/20190515054916/https://www.mkyong.com/tag/struts2/)







