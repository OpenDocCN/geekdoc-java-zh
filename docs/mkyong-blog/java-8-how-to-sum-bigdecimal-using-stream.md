# Java 8–如何使用 Stream 对 BigDecimal 求和？

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java8/java-8-how-to-sum-bigdecimal-using-stream/>

在 Java 8 中，我们可以使用`Stream.reduce()`对一列`BigDecimal`求和。

## 1.Stream.reduce()

使用一个普通的 for 循环和一个`stream.reduce()`对一系列`BigDecimal`值求和的 Java 示例。

JavaBigDecimal.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.util.LinkedList;
import java.util.List;

public class JavaBigDecimal {

    public static void main(String[] args) {

        List<BigDecimal> invoices = new LinkedList<>();
        invoices.add(BigDecimal.valueOf(9.9));
        invoices.add(BigDecimal.valueOf(1.0));
        invoices.add(BigDecimal.valueOf(19.99));
        invoices.add(BigDecimal.valueOf(0.2));
        invoices.add(BigDecimal.valueOf(5.5));

        // sum using a for loop
        BigDecimal sum = BigDecimal.ZERO;
        for (BigDecimal amt : invoices) {
            sum = sum.add(amt);
        }
        System.out.println("Sum = " + sum);

        // sum using stream
        BigDecimal sum2 = invoices.stream().reduce(BigDecimal.ZERO, BigDecimal::add);
        System.out.println("Sum (Stream) = " + sum2);

    }

} 
```

输出

```java
 Sum = 36.59
Sum (Stream) = 36.59 
```

## 2.映射和缩小

对列表`Invoices`中的所有`BigDecimal`求和。

JavaBigDecimalObject.java

```java
 package com.mkyong;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.Arrays;
import java.util.List;

public class JavaBigDecimalObject {

    public static void main(String[] args) {

        List<Invoice> invoices = Arrays.asList(
                new Invoice("I1001", BigDecimal.valueOf(9.99), BigDecimal.valueOf(1)),
                new Invoice("I1002", BigDecimal.valueOf(19.99), BigDecimal.valueOf(1.5)),
                new Invoice("I1003", BigDecimal.valueOf(4.888), BigDecimal.valueOf(2)),
                new Invoice("I1004", BigDecimal.valueOf(4.99), BigDecimal.valueOf(5)),
                new Invoice("I1005", BigDecimal.valueOf(.5), BigDecimal.valueOf(2.3))
        );

        BigDecimal sum = invoices.stream()
                .map(x -> x.getQty().multiply(x.getPrice()))    // map
                .reduce(BigDecimal.ZERO, BigDecimal::add);      // reduce

        System.out.println(sum);    // 75.851
        System.out.println(sum.setScale(2, RoundingMode.HALF_UP));  // 75.85

    }

}

class Invoice {

    String invoiceNo;
    BigDecimal price;
    BigDecimal qty;

    public Invoice(String invoiceNo, BigDecimal price, BigDecimal qty) {
        this.invoiceNo = invoiceNo;
        this.price = price;
        this.qty = qty;
    }

    public String getInvoiceNo() {
        return invoiceNo;
    }

    public void setInvoiceNo(String invoiceNo) {
        this.invoiceNo = invoiceNo;
    }

    public BigDecimal getPrice() {
        return price;
    }

    public void setPrice(BigDecimal price) {
        this.price = price;
    }

    public BigDecimal getQty() {
        return qty;
    }

    public void setQty(BigDecimal qty) {
        this.qty = qty;
    }
} 
```

输出

```java
 75.851
75.85 
```

## 参考

*   [Java 8 Stream.reduce()示例](/web/20221206172253/https://mkyong.com/java8/java-8-stream-reduce-examples/)
*   [如何在 Java 中计算货币值](/web/20221206172253/https://mkyong.com/java/how-do-calculate-monetary-values-in-java-double-vs-bigdecimal/)

<input type="hidden" id="mkyong-current-postId" value="15479">