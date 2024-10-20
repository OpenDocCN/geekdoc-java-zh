# Spring Boot 测试无法自动连线 MockMvc

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/spring-boot/spring-boot-test-unable-to-autowired-mockmvc/>

Spring Boot 积分测试，但无法`@Autowired MockMvc`

*PS 用 Spring Boot 2.1.2.RELEASE 测试*

BookControllerTest.java

```java
 package com.mkyong;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.hamcrest.Matchers.is;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class BookControllerTest {

    @Autowired
    private MockMvc mockMvc; //null?

    @Test
    public void findOne() throws Exception {
        mockMvc.perform(get("/books/1"))
                .andDo(print())
                .andExpect(content().contentType(MediaType.APPLICATION_JSON_UTF8))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id", is(1)));
    }

} 
```

```java
 Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: 
	No qualifying bean of type 'org.springframework.test.web.servlet.MockMvc' available: 
	expected at least 1 bean which qualifies as autowire candidate. 
	Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}

	//...
	... 28 more 
```

## 解决办法

添加此`@AutoConfigureMockMvc`来启用和配置`MockMvc`的自动配置

```java
 package com.mkyong;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.security.test.context.support.WithMockUser;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.hamcrest.Matchers.is;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc //need this in Spring Boot test
public class BookControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void findOne() throws Exception {
        mockMvc.perform(get("/books/1"))
                .andDo(print())
                .andExpect(content().contentType(MediaType.APPLICATION_JSON_UTF8))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id", is(1)));
    }

} 
```

 ## 参考

*   [自动配置模型 Mvc 文档](http://web.archive.org/web/20190308020137/https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/web/servlet/AutoConfigureMockMvc.html)
*   [测试网页层](http://web.archive.org/web/20190308020137/https://spring.io/guides/gs/testing-web/)
*   [测试网页层](http://web.archive.org/web/20190308020137/https://stackoverflow.com/questions/46343782/whats-the-difference-between-autoconfigurewebmvc-and-autoconfiguremockmvc)
*   [@ autoconfigureweb MVC 和@AutoConfigureMockMvc 有什么区别？](http://web.archive.org/web/20190308020137/https://spring.io/guides/gs/testing-web/)

[mock](http://web.archive.org/web/20190308020137/http://www.mkyong.com/tag/mock/) [MockMvc](http://web.archive.org/web/20190308020137/http://www.mkyong.com/tag/mockmvc/) [spring boot](http://web.archive.org/web/20190308020137/http://www.mkyong.com/tag/spring-boot/) [spring test](http://web.archive.org/web/20190308020137/http://www.mkyong.com/tag/spring-test/)![](img/eac3a102052d0d0b015c2587d374d310.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190308020137/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="14929">







