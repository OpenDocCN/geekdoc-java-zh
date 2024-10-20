# spring Test——如何在 jsonPath 中测试 JSON 数组

> 原文：<http://web.archive.org/web/20230101150211/https://www.mkyong.com/spring-boot/spring-test-how-to-test-a-json-array-in-jsonpath/>

在 Spring 中，我们可以使用像`hasItem`和`hasSize`这样的 Hamcrest APIs 来测试 JSON 数组。查看以下示例:

```java
 package com.mkyong;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.hamcrest.Matchers.*;
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.times;
import static org.mockito.Mockito.verify;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
@ActiveProfiles("test")
public class BookControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private BookRepository mockRepository;

    /*
        {
            "timestamp":"2019-03-05T09:34:13.280+0000",
            "status":400,
            "errors":["Author is not allowed.","Please provide a price","Please provide a author"]
        }
     */
    //article : jsonpath in array
    @Test
    public void save_emptyAuthor_emptyPrice_400() throws Exception {

        String bookInJson = "{\"name\":\"ABC\"}";

        mockMvc.perform(post("/books")
                .content(bookInJson)
                .header(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON))
                .andDo(print())
                .andExpect(status().isBadRequest())
                .andExpect(jsonPath("$.timestamp", is(notNullValue())))
                .andExpect(jsonPath("$.status", is(400)))
                .andExpect(jsonPath("$.errors").isArray())
                .andExpect(jsonPath("$.errors", hasSize(3)))
                .andExpect(jsonPath("$.errors", hasItem("Author is not allowed.")))
                .andExpect(jsonPath("$.errors", hasItem("Please provide a author")))
                .andExpect(jsonPath("$.errors", hasItem("Please provide a price")));

        verify(mockRepository, times(0)).save(any(Book.class));

    }

} 
```

用 Spring Boot 2 号进行了测试

## 参考

*   [哈姆克雷斯特](http://web.archive.org/web/20210814204316/http://hamcrest.org/JavaHamcrest/)
*   [弹簧座验证示例](/web/20210814204316/https://mkyong.com/spring-boot/spring-rest-validation-example/)

Tags : [json](http://web.archive.org/web/20210814204316/https://mkyong.com/tag/json/) [json array](http://web.archive.org/web/20210814204316/https://mkyong.com/tag/json-array/) [spring test](http://web.archive.org/web/20210814204316/https://mkyong.com/tag/spring-test/) [unit test](http://web.archive.org/web/20210814204316/https://mkyong.com/tag/unit-test/)<input type="hidden" id="mkyong-current-postId" value="14947">