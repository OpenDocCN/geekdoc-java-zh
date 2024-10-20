# JUnit–忽略测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-4-tutorial-3-ignore-test/>

在 JUnit 中，要忽略一个测试，只需在`@Test`方法之前或之后添加一个`@Ignore`注释。

*P.S .用 JUnit 4.12 测试*

IgnoreTest.java

```java
 package com.mkyong;

import org.junit.Ignore;
import org.junit.Test;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

public class IgnoreTest {

    @Test
    public void testMath1() {
        assertThat(1 + 1, is(2));
    }

    @Ignore
    @Test
    public void testMath2() {
        assertThat(1 + 2, is(5));
    }

    @Ignore("some one please create a test for Math3!")
    @Test
    public void testMath3() {
        //...
    }

} 
```

在上面的例子中，`testMath2()`和`testMath3()`都将被忽略。

## 常见问题（frequently asked questions）

1.要忽略一个测试，为什么不只是注释测试方法或者`@Test`注释呢？
*答:测试运行人员不会报告测试。在 IDE 中，测试运行程序会用不同的图标或颜色显示被忽略的测试，并突出显示，这样您就知道哪些测试被忽略了。*

2.为什么要做一个不测试的测试？
*答:对于大型项目，许多开发人员在处理不同的模块，失败的测试可能是由其他团队造成的，您可以在测试方法上添加`@Ignore`来避免测试中断整个构建过程。*

*答:或者您希望有人帮助创建测试，像`@Ignore ("help for this method!")`一样，可选参数(字符串)将显示在测试运行器中。*

 ## 参考

1.  [JUnit–忽略 JavaDoc](http://web.archive.org/web/20190225092958/http://junit.sourceforge.net/javadoc/org/junit/Ignore.html)

[junit](http://web.archive.org/web/20190225092958/http://www.mkyong.com/tag/junit/) [unit test](http://web.archive.org/web/20190225092958/http://www.mkyong.com/tag/unit-test/)![](img/4f7a6822b3af677ca33abb55f9d4ef9b.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190225092958/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="1391">







