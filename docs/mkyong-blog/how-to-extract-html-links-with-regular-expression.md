# 如何用正则表达式提取 HTML 链接

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/regular-expressions/how-to-extract-html-links-with-regular-expression/>

在本教程中，我们将向您展示如何从 HTML 页面中提取超链接。例如，从以下内容中获取链接:

```java
 this is text1 <a href='mkyong.com' target='_blank'>hello</a> this is text2... 
```

1.  首先从`a`标签获取“值”——结果:`a href='mkyong.com' target='_blank'`
2.  稍后从上面提取的值中获取“链接”——结果:`mkyong.com`

## 1.正则表达式模式

提取标签正则表达式模式

```java
 (?i)<a([^>]+)>(.+?)</a> 
```

从标签正则表达式模式中提取链接

```java
 \s*(?i)href\s*=\s*(\"([^"]*\")|'[^']*'|([^'">\s]+)); 
```

描述

```java
 (		#start of group #1
 ?i		#  all checking are case insensive
)		#end of group #1
<a              #start with "<a"
  (		#  start of group #2
    [^>]+	#     anything except (">"), at least one character
   )		#  end of group #2
  >		#     follow by ">"
    (.+?)	#	match anything 
         </a>	#	  end with "</a> 
```

```java
 \s*			   #can start with whitespace
  (?i)			   # all checking are case insensive
     href		   #  follow by "href" word
        \s*=\s*		   #   allows spaces on either side of the equal sign,
              (		   #    start of group #1
               "([^"]*")   #      allow string with double quotes enclosed - "string"
               |	   #	  ..or
               '[^']*'	   #        allow string with single quotes enclosed - 'string'
               |           #	  ..or
               ([^'">]+)   #      can't contains one single quotes, double quotes ">"
	      )		   #    end of group #1 
```

## 2.Java 链接提取器示例

下面是一个简单的 Java 链接提取器示例，从第一个模式中提取`a`标记值，并使用第二个模式从第一个模式中提取链接。

HTMLLinkExtractor.java

```java
 package com.mkyong.crawler.core;

import java.util.Vector;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class HTMLLinkExtractor {

	private Pattern patternTag, patternLink;
	private Matcher matcherTag, matcherLink;

	private static final String HTML_A_TAG_PATTERN = "(?i)<a([^>]+)>(.+?)</a>";
	private static final String HTML_A_HREF_TAG_PATTERN = 
		"\\s*(?i)href\\s*=\\s*(\"([^\"]*\")|'[^']*'|([^'\">\\s]+))";

	public HTMLLinkExtractor() {
		patternTag = Pattern.compile(HTML_A_TAG_PATTERN);
		patternLink = Pattern.compile(HTML_A_HREF_TAG_PATTERN);
	}

	/**
	 * Validate html with regular expression
	 * 
	 * @param html
	 *            html content for validation
	 * @return Vector links and link text
	 */
	public Vector<HtmlLink> grabHTMLLinks(final String html) {

		Vector<HtmlLink> result = new Vector<HtmlLink>();

		matcherTag = patternTag.matcher(html);

		while (matcherTag.find()) {

			String href = matcherTag.group(1); // href
			String linkText = matcherTag.group(2); // link text

			matcherLink = patternLink.matcher(href);

			while (matcherLink.find()) {

				String link = matcherLink.group(1); // link
				HtmlLink obj = new HtmlLink();
				obj.setLink(link);
				obj.setLinkText(linkText);

				result.add(obj);

			}

		}

		return result;

	}

	class HtmlLink {

		String link;
		String linkText;

		HtmlLink(){};

		@Override
		public String toString() {
			return new StringBuffer("Link : ").append(this.link)
			.append(" Link Text : ").append(this.linkText).toString();
		}

		public String getLink() {
			return link;
		}

		public void setLink(String link) {
			this.link = replaceInvalidChar(link);
		}

		public String getLinkText() {
			return linkText;
		}

		public void setLinkText(String linkText) {
			this.linkText = linkText;
		}

		private String replaceInvalidChar(String link){
			link = link.replaceAll("'", "");
			link = link.replaceAll("\"", "");
			return link;
		}

	}
} 
```

## 3.单元测试

用 TestNG 进行单元测试。通过`@DataProvider`模拟 HTML 内容。

TestHTMLLinkExtractor.java

```java
 package com.mkyong.crawler.core;

import java.util.Vector;

import org.testng.Assert;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

import com.mkyong.crawler.core.HTMLLinkExtractor.HtmlLink;

/**
 * HTML link extrator Testing
 * 
 * @author mkyong
 * 
 */
public class TestHTMLLinkExtractor {

	private HTMLLinkExtractor htmlLinkExtractor;
	String TEST_LINK = "http://www.google.com";

	@BeforeClass
	public void initData() {
		htmlLinkExtractor = new HTMLLinkExtractor();
	}

	@DataProvider
	public Object[][] HTMLContentProvider() {
	  return new Object[][] {
	    new Object[] { "abc hahaha <a href='" + TEST_LINK + "'>google</a>" },
	    new Object[] { "abc hahaha <a HREF='" + TEST_LINK + "'>google</a>" },

	    new Object[] { "abc hahaha <A HREF='" + TEST_LINK + "'>google</A> , "
		+ "abc hahaha <A HREF='" + TEST_LINK + "' target='_blank'>google</A>" },

	    new Object[] { "abc hahaha <A HREF='" + TEST_LINK + "' target='_blank'>google</A>" },
	    new Object[] { "abc hahaha <A target='_blank' HREF='" + TEST_LINK + "'>google</A>" },
	    new Object[] { "abc hahaha <A target='_blank' HREF=\"" + TEST_LINK + "\">google</A>" },
	    new Object[] { "abc hahaha <a HREF=" + TEST_LINK + ">google</a>" }, };
	}

	@Test(dataProvider = "HTMLContentProvider")
	public void ValidHTMLLinkTest(String html) {

		Vector<HtmlLink> links = htmlLinkExtractor.grabHTMLLinks(html);

		//there must have something
		Assert.assertTrue(links.size() != 0);

		for (int i = 0; i < links.size(); i++) {
			HtmlLink htmlLinks = links.get(i);
			//System.out.println(htmlLinks);
			Assert.assertEquals(htmlLinks.getLink(), TEST_LINK);
		}

	}
} 
```

*结果*

```java
 [TestNG] Running:
  /private/var/folders/w8/jxyz5pf51lz7nmqm_hv5z5br0000gn/T/testng-eclipse--530204890/testng-customsuite.xml

PASSED: ValidHTMLLinkTest("abc hahaha <a href='http://www.google.com'>google</a>")
PASSED: ValidHTMLLinkTest("abc hahaha <a HREF='http://www.google.com'>google</a>")
PASSED: ValidHTMLLinkTest("abc hahaha <A HREF='http://www.google.com'>google</A> , abc hahaha <A HREF='http://www.google.com' target='_blank'>google</A>")
PASSED: ValidHTMLLinkTest("abc hahaha <A HREF='http://www.google.com' target='_blank'>google</A>")
PASSED: ValidHTMLLinkTest("abc hahaha <A target='_blank' HREF='http://www.google.com'>google</A>")
PASSED: ValidHTMLLinkTest("abc hahaha <A target='_blank' HREF="http://www.google.com">google</A>")
PASSED: ValidHTMLLinkTest("abc hahaha <a HREF=http://www.google.com>google</a>") 
```

## 参考

1.  [测试文档](http://web.archive.org/web/20210118065340/http://testng.org/doc/documentation-main.html)
2.  [维基中的超链接](http://web.archive.org/web/20210118065340/https://en.wikipedia.org/wiki/Hyperlink)

Tags : [html](http://web.archive.org/web/20210118065340/https://mkyong.com/tag/html/) [regex](http://web.archive.org/web/20210118065340/https://mkyong.com/tag/regex/)<input type="hidden" id="mkyong-current-postId" value="2052">

### 相关文章

*   [如何用正则表达式验证 HTML 标签](/web/20210118065340/https://mkyong.com/regular-expressions/how-to-validate-html-tag-with-regular-expression/)
*   [Java -如何移除字符串之间的空格](/web/20210118065340/https://mkyong.com/java/how-to-remove-whitespace-between-string-java/)
*   [如何在 Java 中验证电话号码(常规表达式](/web/20210118065340/https://mkyong.com/java/how-do-validate-phone-number-in-java-regular-expression/)
*   [Java 中如何转义特殊字符？](/web/20210118065340/https://mkyong.com/java/how-to-escape-special-characters-in-java/)
*   [如何将 Java 源代码转换成 HTML 页面](/web/20210118065340/https://mkyong.com/java/how-to-convert-java-source-code-to-html-page/)

*   [如何将 HTML 转换成 Javascript(。js)在 Java 中](/web/20210118065340/https://mkyong.com/java/how-to-convert-html-to-javascript-js-in-java/)
*   [如何从 Java 执行 shell 命令](/web/20210118065340/https://mkyong.com/java/how-to-execute-shell-command-from-java/)
*   [如何在 Java 中拆分字符串](/web/20210118065340/https://mkyong.com/java/java-how-to-split-a-string/)
*   [Html 教程 Hello World](/web/20210118065340/https://mkyong.com/html/html-tutorial-hello-world/)
*   [Html 颜色教程](/web/20210118065340/https://mkyong.com/html/html-color-tutorial/)