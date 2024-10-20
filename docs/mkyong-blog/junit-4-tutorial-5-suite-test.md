# JUnit–套件测试，运行多个测试用例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-4-tutorial-5-suite-test/>

在 JUnit 中，您可以使用`@RunWith`和`@Suite`注释运行多个测试用例。参考下面的例子:

SuiteAbcTest.java

```java
 package com.mkyong;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({
        Exception1Test.class, //test case 1
        TimeoutTest.class     //test case 2
})
public class SuiteAbcTest {
	//normally, this is an empty class
} 
```

*P.S .用 JUnit 4.12 测试*

## 参考

1.  [JUnit @RunWith JavaDoc](http://web.archive.org/web/20190225100645/http://junit.org/junit4/javadoc/4.12/org/junit/runner/RunWith.html)

[junit](http://web.archive.org/web/20190225100645/http://www.mkyong.com/tag/junit/)![](img/5ca0b5b6dc9e3ce37bf9430a5a004991.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225100645/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1398">







