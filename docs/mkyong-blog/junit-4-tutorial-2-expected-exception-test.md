# JUnit–预期异常测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-4-tutorial-2-expected-exception-test/>

在 JUnit 中，有 3 种方法可以测试预期的异常:

1.  `@Test`，可选的‘预期’属性
2.  尝试-捕捉并总是`fail()`
3.  `@Rule`预期异常

*P.S .用 JUnit 4.12 测试*

## 1.@Test 预期属性

如果您只想测试异常类型，请使用此选项，请参考以下内容:

Exception1Test.java

```java
 package com.mkyong;

import org.junit.Test;
import java.util.ArrayList;

public class Exception1Test {

    @Test(expected = ArithmeticException.class)
    public void testDivisionWithException() {
        int i = 1 / 0;
    }

    @Test(expected = IndexOutOfBoundsException.class)
    public void testEmptyList() {
        new ArrayList<>().get(0);
    }

} 
```

 ## 2.Try-catch 并且总是失败()

这是一个有点老的学校，在 JUnit 3 中广泛使用。测试异常类型和异常详细信息。参考下文:

Exception2Test.java

```java
 package com.mkyong;

import org.junit.Test;
import java.util.ArrayList;
import static junit.framework.TestCase.fail;
import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.MatcherAssert.assertThat;

public class Exception2Test {

    @Test
    public void testDivisionWithException() {
        try {
            int i = 1 / 0;
            fail(); //remember this line, else 'may' false positive
        } catch (ArithmeticException e) {
            assertThat(e.getMessage(), is("/ by zero"));
			//assert others
        }
    }

    @Test
    public void testEmptyList() {
        try {
            new ArrayList<>().get(0);
            fail();
        } catch (IndexOutOfBoundsException e) {
            assertThat(e.getMessage(), is("Index: 0, Size: 0"));
        }
    }

} 
```

**Always remember the fail()!**
If the line you want to test didn’t throw any exception, and you forgot to put the `fail()`, the test will be passed (false positive). ## 3.@Rule 需要异常

这个`ExpectedException`规则(从 JUnit 4.7 开始)允许您测试异常类型和异常细节，类似于" *2。*“试-赶和总是失败()”的方法，但用了一种更优雅的方式:

Exception3Test.java

```java
 package com.mkyong;

import com.mkyong.examples.CustomerService;
import com.mkyong.examples.exception.NameNotFoundException;
import org.junit.Rule;
import org.junit.Test;
import org.junit.rules.ExpectedException;

import static org.hamcrest.CoreMatchers.containsString;
import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.Matchers.hasProperty;

public class Exception3Test {

    @Rule
    public ExpectedException thrown = ExpectedException.none();

    @Test
    public void testDivisionWithException() {

        thrown.expect(ArithmeticException.class);
        thrown.expectMessage(containsString("/ by zero"));

        int i = 1 / 0;

    }

    @Test
    public void testNameNotFoundException() throws NameNotFoundException {

		//test type
        thrown.expect(NameNotFoundException.class);

		//test message
        thrown.expectMessage(is("Name is empty!"));

        //test detail
        thrown.expect(hasProperty("errCode"));  //make sure getters n setters are defined.
        thrown.expect(hasProperty("errCode", is(666)));

        CustomerService cust = new CustomerService();
        cust.findByName("");

    }

} 
```

NameNotFoundException.java

```java
 package com.mkyong.examples.exception;

public class NameNotFoundException extends Exception {

    private int errCode;

    public NameNotFoundException(int errCode, String message) {
        super(message);
        this.errCode = errCode;
    }

    public int getErrCode() {
        return errCode;
    }

    public void setErrCode(int errCode) {
        this.errCode = errCode;
    }
} 
```

CustomerService.java

```java
 package com.mkyong.examples;

import com.mkyong.examples.exception.NameNotFoundException;

public class CustomerService {

    public Customer findByName(String name) throws NameNotFoundException {

        if ("".equals(name)) {
            throw new NameNotFoundException(666, "Name is empty!");
        }

        return new Customer(name);

    }

} 
```

## 参考

1.  [JUnit Wiki 异常测试](http://web.archive.org/web/20190224205410/https://github.com/junit-team/junit4/wiki/Exception-testing)
2.  [Java 自定义异常示例](http://web.archive.org/web/20190224205410/http://www.mkyong.com/java/java-custom-exception-examples/)

[junit](http://web.archive.org/web/20190224205410/http://www.mkyong.com/tag/junit/) [unit test](http://web.archive.org/web/20190224205410/http://www.mkyong.com/tag/unit-test/)







