# JUnit 5 嵌套测试

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/junit5/junit-5-nested-test-examples/>

![junit 5 nested test](img/9c3e98674884ca44d87e9ea395514749.png)

本文向您展示了如何使用 JUnit 5 `@Nested`注释将测试分组在一起。

*用 JUnit 5.5.2 测试的 PS*

为什么是嵌套测试？
创建嵌套测试是可选的。尽管如此，创建层次化的上下文来将相关的单元测试组织在一起还是有帮助的；简而言之，它有助于保持测试的整洁和可读性。

让我们看看下面的例子——对一个`CustomerService`的测试。

## 1.单一测试类别

1.1 默认情况下，我们可以在一个类中创建所有的测试，如下所示:

CustomerServiceMethodTest.java

```java
 package com.mkyong.nested.samples;

import com.mkyong.customer.service.CustomerService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("Test Customer Service")
public class CustomerServiceMethodTest {

    CustomerService customerService;

    @BeforeEach
    void createNewObjectForAll() {
        System.out.println("New CustomerService()");
        //customerService = new CustomerServiceJDBC();
    }

    @Test
    void findOne_with_id() {
        //customerService.findOneById(2L);
    }

    @Test
    void findOne_with_name() {
        //customerService.findOneByName(2L);
    }

    @Test
    void findOne_with_name_regex() {
        //customerService.findOneByNameRegex("%s");
    }

    @Test
    void findAll_with_ids() {
        //customerService.findAllByIds(Arrays.asList(2, 3, 4));
    }

    @Test
    void findAll_with_name_like() {
        //customerService.findAllByName("mkyong");
    }

    @Test
    void update_with_new() {
        //customerService.update(new Customer());
    }

    @Test
    void update_with_existing() {
        //customerService.update(new Customer());
    }

} 
```

IDE 中的输出。

![tests in ide](img/b64116968ebe62e1bd664e2811ae128c.png)

如果`CustomerService`增加了更多的特性，那么这个测试类将很容易被数百个测试方法所超载。最后，我们创建了一个混乱的、有意义的、无组织的单一测试类。

## 2.分类测试

2.1 一些开发人员开始按照类名对相关测试进行分组，如下所示:

CustomerServiceFindOneTest.java

```java
 package com.mkyong.nested.samples;

import com.mkyong.customer.service.CustomerService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class CustomerServiceFindOneTest {

    CustomerService customerService;

    @BeforeEach
    void createNewObjectForAll() {
        System.out.println("New CustomerService()");
        //customerService = new CustomerServiceJDBC();
    }

    @Test
    void findOne_with_id() {
        //customerService.findOneById(2L);
    }

    @Test
    void findOne_with_name() {
        //customerService.findOneByName(2L);
    }

    @Test
    void findOne_with_name_regex() {
        //customerService.findOneByNameRegex("%s");
    }

} 
```

CustomerServiceFindAllTest.java

```java
 package com.mkyong.nested.samples;

import com.mkyong.customer.service.CustomerService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class CustomerServiceFindAllTest {

    CustomerService customerService;

    @BeforeEach
    void createNewObjectForAll() {
        System.out.println("New CustomerService()");
        //customerService = new CustomerServiceJDBC();
    }

    @Test
    void findAll_with_ids() {
        //customerService.findAllByIds(Arrays.asList(2, 3, 4));
    }

    @Test
    void findAll_with_name_likeY() {
        //customerService.findAllByName("mkyong");
    }

} 
```

CustomerServiceUpdateTest.java

```java
 package com.mkyong.nested.samples;

import com.mkyong.customer.service.CustomerService;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class CustomerServiceUpdateTest {

    CustomerService customerService;

    @BeforeEach
    void createNewObjectForAll() {
        System.out.println("New CustomerService()");
        //customerService = new CustomerServiceJDBC();
    }

    @Test
    void update_with_new() {
        //customerService.update(new Customer());
    }

    @Test
    void update_with_existing() {
        //customerService.update(new Customer());
    }

} 
```

如果`CustomerService`增加了更多的特性，例如，新的 30 多个方法，我们会为`CustomerService`创建 30 多个测试类吗？如何运行 30 多个测试类，通过模式还是创建一个新的测试套件？

## 3.嵌套测试

3.1 对于大类，我们应该考虑`@Nested`测试，单个测试类中的所有测试(在一个层次结构中)，IDE 中的层次输出使测试更具可读性。

此外，我们还可以初始化一个对象，并对所有嵌套的测试进行重用。

CustomerServiceNestedTest.java

```java
 package com.mkyong.nested;

import com.mkyong.customer.service.CustomerService;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;

@DisplayName("Test Customer Service")
public class CustomerServiceNestedTest {

    CustomerService customerService;

    // Create one customerService object and reuse for all the nested tests
    @Test
    @DisplayName("new CustomerService() for all the nested methods.")
    void createNewObjectForAll() {
        System.out.println("New CustomerService()");
        //customerService = new CustomerServiceJDBC();
    }

    @Nested
    @DisplayName("findOne methods")
    class FindOne {
        @Test
        void findOne_with_id() {
            //customerService.findOneById(2L);
        }

        @Test
        void findWith_with_name() {
            //customerService.findOneByName(2L);
        }

        @Test
        void findWith_with_name_regex() {
            //customerService.findOneByNameRegex("%s");
        }
    }

    @Nested
    @DisplayName("findAll methods")
    class FindAll {
        @Test
        void findAll_with_ids() {
            //customerService.findAllByIds(Arrays.asList(2, 3, 4));
        }

        @Test
        void findAll_with_name_likeY() {
            //customerService.findAllByName("mkyong");
        }
    }

    @Nested
    @DisplayName("update methods")
    class Update {
        @Test
        void update_with_new() {
            //customerService.update(new Customer());
        }

        @Test
        void update_with_existing() {
            //customerService.update(new Customer());
        }
    }

} 
```

IDE 中的输出。

![nested tests in ide](img/c61055c9dda791a4017462c55d623fac.png)

## 下载源代码

$ git clone [https://github.com/mkyong/junit-examples](http://web.archive.org/web/20220617181004/https://github.com/mkyong/junit-examples)
$ cd junit5-examples
$ check src/test/java/com/mkyong/nested/*.java

## 参考

*   [JUnit 5 嵌套测试](http://web.archive.org/web/20220617181004/https://junit.org/junit5/docs/current/user-guide/#writing-tests-nested)
*   [JUnit hierarchical contextrunner Wiki](http://web.archive.org/web/20220617181004/https://github.com/bechte/junit-hierarchicalcontextrunner/wiki)

<input type="hidden" id="mkyong-current-postId" value="15250">