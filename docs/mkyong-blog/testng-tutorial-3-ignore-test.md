# TestNG–如何忽略一个测试方法

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-tutorial-3-ignore-test/>

在本教程中，我们将向您展示如何用`@Test(enabled = false)`忽略一个测试方法。

TestIgnore.java

```java
 package com.mkyong.testng.examples.ignore;

import org.testng.Assert;
import org.testng.annotations.Test;

public class TestIgnore {

	@Test //default enable=true
	public void test1() {
		Assert.assertEquals(true, true);
	}

	@Test(enabled = true)
	public void test2() {
		Assert.assertEquals(true, true);
	}

	@Test(enabled = false)
	public void test3() {
		Assert.assertEquals(true, true);
	}

} 
```

输出

```java
 [TestNG] Running:

PASSED: test1
PASSED: test2

===============================================
    Default test
    Tests run: 2, Failures: 0, Skips: 0
=============================================== 
```

在上面的例子中，`test3 ()`测试方法被忽略。

[ignore](http://web.archive.org/web/20190227120518/http://www.mkyong.com/tag/ignore/) [testng](http://web.archive.org/web/20190227120518/http://www.mkyong.com/tag/testng/)![](img/bea8ea831b65ab04f396f5d7b5fb0a27.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190227120518/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1354">







