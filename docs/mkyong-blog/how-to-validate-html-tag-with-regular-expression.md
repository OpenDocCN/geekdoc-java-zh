# 如何用正则表达式验证 HTML 标签

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/regular-expressions/how-to-validate-html-tag-with-regular-expression/>

HTML 标记正则表达式模式

```java
 <("[^"]*"|'[^']*'|[^'">])*> 
```

描述

```java
 <	  	#start with opening tag "<"
 (		#   start of group #1
   "[^"]*"	#	allow string with double quotes enclosed - "string"
   |		#	..or
   '[^']*'	#	allow string with single quote enclosed - 'string'
   |		#	..or
   [^'">]	#	cant contains one single quotes, double quotes and ">"
 )		#   end of group #1
 *		# 0 or more
>		#end with closing tag ">" 
```

HTML 标签，以开始标签"开始，不带单引号或双引号。最后，以结束标记“>”结束

## Java 正则表达式示例

```java
 package com.mkyong.regex;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class HTMLTagValidator{

   private Pattern pattern;
   private Matcher matcher;

   private static final String HTML_TAG_PATTERN = "<(\"[^\"]*\"|'[^']*'|[^'\">])*>";

   public HTMLTagValidator(){
	  pattern = Pattern.compile(HTML_TAG_PATTERN);
   }

  /**
   * Validate html tag with regular expression
   * @param tag html tag for validation
   * @return true valid html tag, false invalid html tag
   */
  public boolean validate(final String tag){

	  matcher = pattern.matcher(tag);
	  return matcher.matches();

  }
} 
```

 ## 匹配 HTML 标签:

1.**、<input value="’">' >
二<输入值=' < ' >"，"< b/ > "
3。"<a href = ' http://www . Google . com '>
4< br >，<br/>
5。"<输入值=\"\" id='test' >"，"<输入值=" id='test' >"**

 ## HTML 标记不匹配:

1."<input value="\”" id="’test’">"–不允许使用一个双引号字符串
2。"<输入值= ' id = ' test '>"–不允许使用单引号字符串
3。"<输入值=>>"–不允许单引号>，必须用单引号或双引号括起来

## 单元测试–HTMLTagValidatorTest

```java
 package com.mkyong.regex;

import org.testng.Assert;
import org.testng.annotations.*;

/**
 * HTMLTag validator Testing
 * @author mkyong
 *
 */
public class HTMLTagValidatorTest {

	private HTMLTagValidator htmlTagValidator;

	@BeforeClass
        public void initData(){
		htmlTagValidator = new HTMLTagValidator();
        }

	@DataProvider
	public Object[][] ValidHTMLTagProvider() {
    	   return new Object[][]{
		   new Object[] {"<b>"}, 
                   new Object[] {"<input value='>'>"},
		   new Object[] {"<input value='<'>"}, 
		   new Object[] {"<b/>"},
                   new Object[] {"<a href='http://www.google.com'>"},
		   new Object[] {"<br>"},
                   new Object[] {"<br/>"},
		   new Object[] {"<input value=\"\" id='test'>"},
                   new Object[] {"<input value='' id='test'>"}
	   };
	}

	@DataProvider
	public Object[][] InvalidHTMLTagProvider() {
	    return new Object[][]{
		  new Object[] {"<input value=\" id='test'>"},
	  	  new Object[] {"<input value=' id='test'>"},
	  	  new Object[] {"<input value=> >"}
	    };
	}

	@Test(dataProvider = "ValidHTMLTagProvider")
	public void ValidHTMLTagTest(String tag) {

	    boolean valid = htmlTagValidator.validate(tag);
	    System.out.println("HTMLTag is valid : " + tag + " , " + valid);
	    Assert.assertEquals(true, valid);

	}

	@Test(dataProvider = "InvalidHTMLTagProvider", 
                 dependsOnMethods="ValidHTMLTagTest")
	public void InValidHTMLTagTest(String tag) {

	   boolean valid = htmlTagValidator.validate(tag);
	   System.out.println("HTMLTag is valid : " + tag + " , " + valid);
	   Assert.assertEquals(false, valid);

	}
} 
```

## 单元测试–结果

```java
 HTMLTag is valid : <b> , true
HTMLTag is valid : <input value='>'> , true
HTMLTag is valid : <input value='<'> , true
HTMLTag is valid : <b/> , true
HTMLTag is valid : <a href='http://www.google.com'> , true
HTMLTag is valid : <br> , true
HTMLTag is valid : <br/> , true
HTMLTag is valid : <input value="" id='test'> , true
HTMLTag is valid : <input value='' id='test'> , true
HTMLTag is valid : <input value=" id='test'> , false
HTMLTag is valid : <input value=' id='test'> , false
HTMLTag is valid : <input value=> > , false
PASSED: ValidHTMLTagTest("<b>")
PASSED: ValidHTMLTagTest("<input value='>'>")
PASSED: ValidHTMLTagTest("<input value='<'>")
PASSED: ValidHTMLTagTest("<b/>")
PASSED: ValidHTMLTagTest("<a href='http://www.google.com'>")
PASSED: ValidHTMLTagTest("<br>")
PASSED: ValidHTMLTagTest("<br/>")
PASSED: ValidHTMLTagTest("<input value="" id='test'>")
PASSED: ValidHTMLTagTest("<input value='' id='test'>")
PASSED: InValidHTMLTagTest("<input value=" id='test'>")
PASSED: InValidHTMLTagTest("<input value=' id='test'>")
PASSED: InValidHTMLTagTest("<input value=> >")

===============================================
    com.mkyong.regex.HTMLTagValidatorTest
    Tests run: 12, Failures: 0, Skips: 0
===============================================

===============================================
mkyong
Total tests run: 12, Failures: 0, Skips: 0
=============================================== 
```

想了解更多关于正则表达式的知识吗？强烈推荐这本最好最经典的书——《掌握正则表达式》

<center>

[http://web.archive.org/web/20190310100540if_/http://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr](http://web.archive.org/web/20190310100540if_/http://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr)

</center>

[html](http://web.archive.org/web/20190310100540/http://www.mkyong.com/tag/html/) [regex](http://web.archive.org/web/20190310100540/http://www.mkyong.com/tag/regex/)







