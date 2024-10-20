# TestNG–超时测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/testng-tutorial-4-time-test/>

在本教程中，我们将向您展示如何在 TestNG 中执行`timeout`测试。“超时”意味着如果一个单元测试花费的时间超过了指定的毫秒数，TestNG 将会中止它，并将其作为失败处理。

这个“超时”也可以用于性能测试，以确保方法在合理的时间内返回。

TestTimeout.java

```java
 package com.mkyong.testng.examples.timeout;

import org.testng.annotations.Test;

public class TestTimeout {

	@Test(timeOut = 5000) // time in mulliseconds
	public void testThisShouldPass() throws InterruptedException {
		Thread.sleep(4000);
	}

	@Test(timeOut = 1000)
	public void testThisShouldFail() {
		while (true);
	}

} 
```

输出

```java
 [TestNG] Running:

PASSED: testThisShouldPass
FAILED: testThisShouldFail
org.testng.internal.thread.ThreadTimeoutException: Method org.testng.internal.TestNGMethod.testThisShouldFail() didn't finish within the time-out 1000
	at com.mkyong.testng.examples.timeout.TestTimeout.testThisShouldFail(TestTimeout.java:14)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
	at java.lang.reflect.Method.invoke(Unknown Source)
	at org.testng.internal.MethodInvocationHelper.invokeMethod(MethodInvocationHelper.java:84)
	at org.testng.internal.InvokeMethodRunnable.runOne(InvokeMethodRunnable.java:46)
	at org.testng.internal.InvokeMethodRunnable.run(InvokeMethodRunnable.java:37)
	at java.util.concurrent.Executors$RunnableAdapter.call(Unknown Source)
	at java.util.concurrent.FutureTask.run(Unknown Source)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
	at java.lang.Thread.run(Unknown Source)

===============================================
    Default test
    Tests run: 2, Failures: 1, Skips: 0
=============================================== 
```

[testng](http://web.archive.org/web/20190227120240/http://www.mkyong.com/tag/testng/) [timeout](http://web.archive.org/web/20190227120240/http://www.mkyong.com/tag/timeout/)![](img/c306c26b3b675eac2365691f1a4e54ae.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190227120240/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1358">







