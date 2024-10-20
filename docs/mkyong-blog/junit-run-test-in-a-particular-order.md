# JUnit——以特定的顺序运行测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-run-test-in-a-particular-order/>

在 JUnit 中，您可以使用`@FixMethodOrder(MethodSorters.NAME_ASCENDING)`按照方法名，按字典顺序运行测试方法。

*P.S .用 JUnit 4.12 测试*

ExecutionOrderTest.java

```java
 package com.mkyong;

import org.junit.FixMethodOrder;
import org.junit.Test;
import org.junit.runners.MethodSorters;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

//Sorts by method name
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class ExecutionOrderTest {

    @Test
    public void testB() {

        assertThat(1 + 1, is(2));

    }

    @Test
    public void test1() {

        assertThat(1 + 1, is(2));

    }

    @Test
    public void testA() {

        assertThat(1 + 1, is(2));

    }

    @Test
    public void test2() {

        assertThat(1 + 1, is(2));

    }

    @Test
    public void testC() {

        assertThat(1 + 1, is(2));

    }

} 
```

输出，上述测试方法将按以下顺序运行:

```java
test1
test2
testA
testB
testC

```

![junit-execution-order](img/8a580b4aa7e42393dd86dc733fd044db.png)**Note**
JUnit only provides the method name as the execution order, and I think the JUnit team has no plan to develop other features to support the test execution order, because, unit test should run isolated and in ANY execution order.

如果你真的需要测试执行顺序，试试 [TestNG 依赖测试](http://web.archive.org/web/20190224205421/http://www.mkyong.com/unittest/testng-tutorial-7-dependency-test/)

## 参考

1.  [JUnit MethodSorters JavaDoc](http://web.archive.org/web/20190224205421/http://junit.org/junit4/javadoc/4.12/org/junit/runners/MethodSorters.html)
2.  [JUnit 测试执行顺序](http://web.archive.org/web/20190224205421/https://github.com/junit-team/junit4/wiki/Test-execution-order)

[junit](http://web.archive.org/web/20190224205421/http://www.mkyong.com/tag/junit/)![](img/3aaa46d87e9bdf63f9c0b5e2f8005a01.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190224205421/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="14001">







