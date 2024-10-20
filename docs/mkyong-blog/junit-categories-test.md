# JUnit–类别测试

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/unittest/junit-categories-test/>

在 JUnit 中，您可以将测试用例组织成不同的类别，并使用`@Categories.ExcludeCategory`或`@Categories.IncludeCategory`运行这些分类的测试用例

**Note**
This `@Categories` annotation is available since JUnit 4.12

## 1.类别=标记界面

在 JUnit 中，您需要创建标记接口来表示类别:

PerformanceTests.java

```java
 package com.mkyong.category;

//category marker interface
public interface PerformanceTests {
} 
```

RegressionTests.java

```java
 package com.mkyong.category;

public interface RegressionTests {
} 
```

 ## 2.@类别示例

将测试用例组织成不同的类别。

2.1 方法级别的`@Category`。

ClassA.java

```java
 package com.mkyong.category;

import org.junit.Test;
import org.junit.experimental.categories.Category;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

public class ClassA {

    @Category(PerformanceTests.class)
    @Test
    public void test_a_1() {
        assertThat(1 == 1, is(true));
    }

    @Test
    public void test_a_2() {
        assertThat(1 == 1, is(true));
    }

} 
```

2.2 `@Category`在班级层面。

ClassB.java

```java
 package com.mkyong.category;

import org.junit.Test;
import org.junit.experimental.categories.Category;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

@Category({PerformanceTests.class, RegressionTests.class})
public class ClassB {

    @Test
    public void test_b_1() {
        assertThat(1 == 1, is(true));
    }

} 
```

2.3 多个`@Category`例子。

ClassC.java

```java
 package com.mkyong.category;

import org.junit.Test;
import org.junit.experimental.categories.Category;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

public class ClassC {

    @Category({PerformanceTests.class, RegressionTests.class})
    @Test
    public void test_c_1() {
        assertThat(1 == 1, is(true));
    }

    @Category(RegressionTests.class)
    @Test
    public void test_c_2() {
        assertThat(1 == 1, is(true));
    }

} 
```

 ## 3.套件测试

运行分类测试用例的例子。

3.1 包含类别示例，运行`PerformanceTests`类别。

PerformanceTestSuite.java

```java
 package com.mkyong.category;

import org.junit.experimental.categories.Categories;
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Categories.class)
@Categories.IncludeCategory(PerformanceTests.class)
//Include multiple categories
//@Categories.IncludeCategory({PerformanceTests.class, RegressionTests.class})
@Suite.SuiteClasses({ClassA.class, ClassB.class, ClassC.class})
public class PerformanceTestSuite {
} 
```

输出

```java
ClassA.test_a_1()
ClassB.test_b_1()
ClassC.test_c_1()

```

3.2 包含类别示例，运行`RegressionTestSuite`类别。

RegressionTestSuite.java

```java
 package com.mkyong.category;

import org.junit.experimental.categories.Categories;
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Categories.class)
@Categories.IncludeCategory(RegressionTests.class)
@Suite.SuiteClasses({ClassA.class, ClassB.class, ClassC.class})
public class RegressionTestSuite {
} 
```

输出

```java
ClassB.test_b_1()
ClassC.test_c_1()
ClassC.test_c_2()

```

3.3 排除类别示例。

ExcludePerformanceTestSuite.java

```java
 package com.mkyong.category;

import org.junit.experimental.categories.Categories;
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Categories.class)
@Categories.ExcludeCategory(PerformanceTests.class)
@Suite.SuiteClasses({ClassA.class, ClassB.class, ClassC.class})
public class ExcludePerformanceTestSuite {
} 
```

输出

```java
ClassA.test_a_2()
ClassC.test_c_2()

```

**Note**
This is similar with the [TestNG group test](http://web.archive.org/web/20190223082759/http://www.mkyong.com/unittest/testng-groups-example/).

## 参考

1.  [JUnit 类别 JavaDoc](http://web.archive.org/web/20190223082759/http://junit.org/junit4/javadoc/4.12/org/junit/experimental/categories/Categories.html)
2.  [JUnit Wiki–类别](http://web.archive.org/web/20190223082759/https://github.com/junit-team/junit4/wiki/Categories)
3.  [TestNG 组测试](http://web.archive.org/web/20190223082759/http://www.mkyong.com/unittest/testng-groups-example/)

[category test](http://web.archive.org/web/20190223082759/http://www.mkyong.com/tag/category-test/) [group test](http://web.archive.org/web/20190223082759/http://www.mkyong.com/tag/group-test/) [junit](http://web.archive.org/web/20190223082759/http://www.mkyong.com/tag/junit/)







