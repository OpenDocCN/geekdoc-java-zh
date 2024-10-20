# mock ITO–when()需要一个必须是“模拟上的方法调用”的参数

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/mockito-when-requires-an-argument-which-has-to-be-a-method-call-on-a-mock/>

运行下面的 Spring Boot + JUnit 5 + Mockito 集成测试。

```java
 package com.mkyong.core.services;

import com.mkyong.core.repository.HelloRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.springframework.boot.test.context.SpringBootTest;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

@SpringBootTest
public class HelloServiceMockTest {

    @Mock
    private HelloRepository helloRepository;

    @InjectMocks // auto inject helloRepository
    private HelloService helloService = new HelloServiceImpl();

    @BeforeEach
    void setMockOutput() {
        when(helloService.getDefault()).thenReturn("Hello Mockito");
    }

	//...
} 
```

但是会遇到以下错误:

```java
 when() requires an argument which has to be 'a method call on a mock'.
For example:
    when(mock.getArticles()).thenReturn(articles);

Also, this error might show up because:
1\. you stub either of: final/private/equals()/hashCode() methods.
   Those methods <strong>cannot</strong> be stubbed/verified.
   Mocking methods declared on non-public parent classes is not supported.
2\. inside when() you don't call method on mock but on some other object. 
```

## 解决办法

这个`when(mock.getArticles()).thenReturn(articles);`需要应用在被模仿的对象上。当 Mockito 看到这个`@InjectMocks`时，它并没有嘲笑它，它只是创建了一个普通的实例，所以这个`when()`将会失败。

要解决它，注释`@spy`来部分嘲讽它。

```java
 package com.mkyong.core.services;

import com.mkyong.core.repository.HelloRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.Spy;
import org.springframework.boot.test.context.SpringBootTest;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.when;

@SpringBootTest
public class HelloServiceMockTest {

    @Mock
    private HelloRepository helloRepository;

    @Spy // mock it partially
    @InjectMocks
    private HelloService helloService = new HelloServiceImpl();

    @BeforeEach
    void setMockOutput() {
        when(helloService.getDefault()).thenReturn("Hello Mockito");
    }

	//...
} 
```

# 参考

*   [Spring Boot +朱尼特 5 +莫奇托](http://web.archive.org/web/20190308020415/https://www.mkyong.com/spring-boot/spring-boot-junit-5-mockito/)
*   莫克西托

[mock](http://web.archive.org/web/20190308020415/http://www.mkyong.com/tag/mock/) [mockito](http://web.archive.org/web/20190308020415/http://www.mkyong.com/tag/mockito/) [spring boot](http://web.archive.org/web/20190308020415/http://www.mkyong.com/tag/spring-boot/)![](img/c6de9842631264e61e9422f8af18f623.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190308020415/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="14916">







