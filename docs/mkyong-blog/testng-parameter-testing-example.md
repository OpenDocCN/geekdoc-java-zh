# TestNG 参数测试示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-parameter-testing-example/>

又一个 TestNG 参数测试例子，用`@DataProvider`。

## 1.CharUtil 类

比方说，一个将字符转换为 ASCII 或相反的类，如何用 TestNG 对它进行单元测试？

CharUtils.java

```java
 package com.mkyong.testng.examples.parameter;

/**
 * Character Utility class
 * 
 * @author mkyong
 * 
 */
public class CharUtils {
	/**
	 * Convert the characters to ASCII value
	 * 
	 * @param character character
	 * @return ASCII value
	 */
	public static int CharToASCII(final char character) {
		return (int) character;
	}

	/**
	 * Convert the ASCII value to character
	 * 
	 * @param ascii ascii value
	 * @return character value
	 */
	public static char ASCIIToChar(final int ascii) {
		return (char) ascii;
	}
} 
```

 ## 2.TestNG @DataProvider 示例

为了测试它，创建一个接受两个参数(字符和期望的 ASCII)的`@Test`方法，测试数据从数据提供者传递。

CharUtilsTest.java

```java
 package com.mkyong.testng.examples.parameter;

import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;
/**
 * Character Utils Testing
 * @author mkyong
 *
 */
public class CharUtilsTest {

	@DataProvider
	public Object[][] ValidDataProvider() {
		return new Object[][]{
			{ 'A', 65 },{ 'a', 97 },
			{ 'B', 66 },{ 'b', 98 },
			{ 'C', 67 },{ 'c', 99 },
			{ 'D', 68 },{ 'd', 100 },
			{ 'Z', 90 },{ 'z', 122 },
			{ '1', 49 },{ '9', 57 }
		};
	}

	@Test(dataProvider = "ValidDataProvider")
	public void CharToASCIITest(final char character, final int ascii) {

		   int result = CharUtils.CharToASCII(character); 
		   Assert.assertEquals(result, ascii);

	}

	@Test(dataProvider = "ValidDataProvider")
	public void ASCIIToCharTest(final char character, final int ascii) {

		   char result = CharUtils.ASCIIToChar(ascii); 
		   Assert.assertEquals(result, character); 

	}
} 
```

结果

```java
 PASSED: CharToASCIITest(A, 65)
PASSED: CharToASCIITest(a, 97)
PASSED: CharToASCIITest(B, 66)
PASSED: CharToASCIITest(b, 98)
PASSED: CharToASCIITest(C, 67)
PASSED: CharToASCIITest(c, 99)
PASSED: CharToASCIITest(D, 68)
PASSED: CharToASCIITest(d, 100)
PASSED: CharToASCIITest(Z, 90)
PASSED: CharToASCIITest(z, 122)
PASSED: CharToASCIITest(1, 49)
PASSED: CharToASCIITest(9, 57)
PASSED: ASCIIToCharTest(A, 65)
PASSED: ASCIIToCharTest(a, 97)
PASSED: ASCIIToCharTest(B, 66)
PASSED: ASCIIToCharTest(b, 98)
PASSED: ASCIIToCharTest(C, 67)
PASSED: ASCIIToCharTest(c, 99)
PASSED: ASCIIToCharTest(D, 68)
PASSED: ASCIIToCharTest(d, 100)
PASSED: ASCIIToCharTest(Z, 90)
PASSED: ASCIIToCharTest(z, 122)
PASSED: ASCIIToCharTest(1, 49)
PASSED: ASCIIToCharTest(9, 57)

===============================================
    com.mkyong.common.CharUtilsTest
    Tests run: 24, Failures: 0, Skips: 0
=============================================== 
```

**More Parameter Examples.**
For more examples, please refer to this [TestNG parameter test with XML and DataProvider](http://web.archive.org/web/20190227120414/http://www.mkyong.com/unittest/testng-tutorial-6-parameterized-test/).[parameter test](http://web.archive.org/web/20190227120414/http://www.mkyong.com/tag/parameter-test/) [testng](http://web.archive.org/web/20190227120414/http://www.mkyong.com/tag/testng/)![](img/d9efcf273d2dd5a1eaf8556f42895918.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190227120414/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="2191">







