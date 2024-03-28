# 默认方法

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html`](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)

接口部分描述了一个涉及计算机控制汽车制造商发布行业标准接口的示例，描述了可以调用哪些方法来操作他们的汽车。如果这些计算机控制汽车制造商为他们的汽车添加新功能，比如飞行，会怎么样？这些制造商需要指定新的方法来使其他公司（如电子导航仪制造商）能够调整他们的软件以适应飞行汽车。这些汽车制造商会在哪里声明这些新的与飞行相关的方法？如果他们将它们添加到原始接口中，那么已经实现这些接口的程序员将不得不重新编写他们的实现。如果将它们添加为静态方法，那么程序员会将它们视为实用方法，而不是必要的核心方法。

默认方法使您能够向库的接口添加新功能，并确保与为旧版本接口编写的代码的二进制兼容性。

考虑下面的接口，`TimeClient`，如问题和练习的答案：接口中所述：

```java

import java.time.*; 

public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second);
    LocalDateTime getLocalDateTime();
}

```

下面的类，`SimpleTimeClient`，实现了`TimeClient`：

```java

package defaultmethods;

import java.time.*;
import java.lang.*;
import java.util.*;

public class SimpleTimeClient implements TimeClient {

    private LocalDateTime dateAndTime;

    public SimpleTimeClient() {
        dateAndTime = LocalDateTime.now();
    }

    public void setTime(int hour, int minute, int second) {
        LocalDate currentDate = LocalDate.from(dateAndTime);
        LocalTime timeToSet = LocalTime.of(hour, minute, second);
        dateAndTime = LocalDateTime.of(currentDate, timeToSet);
    }

    public void setDate(int day, int month, int year) {
        LocalDate dateToSet = LocalDate.of(day, month, year);
        LocalTime currentTime = LocalTime.from(dateAndTime);
        dateAndTime = LocalDateTime.of(dateToSet, currentTime);
    }

    public void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second) {
        LocalDate dateToSet = LocalDate.of(day, month, year);
        LocalTime timeToSet = LocalTime.of(hour, minute, second); 
        dateAndTime = LocalDateTime.of(dateToSet, timeToSet);
    }

    public LocalDateTime getLocalDateTime() {
        return dateAndTime;
    }

    public String toString() {
        return dateAndTime.toString();
    }

    public static void main(String... args) {
        TimeClient myTimeClient = new SimpleTimeClient();
        System.out.println(myTimeClient.toString());
    }
}

```

假设您想要向`TimeClient`接口添加新功能，比如通过[`ZonedDateTime`](https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html)对象（类似于[`LocalDateTime`](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)对象，但它存储时区信息）指定时区的能力：

```java
public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
        int hour, int minute, int second);
    LocalDateTime getLocalDateTime();                           
    ZonedDateTime getZonedDateTime(String zoneString);
}

```

对`TimeClient`接口进行这种修改后，您还需要修改`SimpleTimeClient`类并实现`getZonedDateTime`方法。但是，与其将`getZonedDateTime`留空（如前面的例子中），您可以定义一个*默认实现*。（请记住，抽象方法是声明而没有实现的方法。）

```java

package defaultmethods;

import java.time.*;

public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second);
    LocalDateTime getLocalDateTime();

    static ZoneId getZoneId (String zoneString) {
        try {
            return ZoneId.of(zoneString);
        } catch (DateTimeException e) {
            System.err.println("Invalid time zone: " + zoneString +
                "; using default time zone instead.");
            return ZoneId.systemDefault();
        }
    }

    default ZonedDateTime getZonedDateTime(String zoneString) {
        return ZonedDateTime.of(getLocalDateTime(), getZoneId(zoneString));
    }
}

```

您可以在接口中的方法签名开头使用`default`关键字来指定一个方法定义是默认方法。接口中的所有方法声明，包括默认方法，都隐式地是`public`的，因此您可以省略`public`修饰符。

使用这个接口，你不需要修改`SimpleTimeClient`类，而这个类（以及任何实现`TimeClient`接口的类）将已经定义好`getZonedDateTime`方法。下面的例子，`TestSimpleTimeClient`，调用了`SimpleTimeClient`实例的`getZonedDateTime`方法：

```java

package defaultmethods;

import java.time.*;
import java.lang.*;
import java.util.*;

public class TestSimpleTimeClient {
    public static void main(String... args) {
        TimeClient myTimeClient = new SimpleTimeClient();
        System.out.println("Current time: " + myTimeClient.toString());
        System.out.println("Time in California: " +
            myTimeClient.getZonedDateTime("Blah blah").toString());
    }
}

```

## 扩展包含默认方法的接口

当您扩展包含默认方法的接口时，可以执行以下操作：

+   完全不提及默认方法，让您扩展的接口继承默认方法。

+   重新声明默认方法，使其为`abstract`。

+   重新定义默认方法，覆盖它。

假设您扩展了接口`TimeClient`如下：

```java
public interface AnotherTimeClient extends TimeClient { }

```

任何实现接口`AnotherTimeClient`的类都将具有默认方法`TimeClient.getZonedDateTime`指定的实现。

假设您扩展了接口`TimeClient`如下：

```java
public interface AbstractZoneTimeClient extends TimeClient {
    public ZonedDateTime getZonedDateTime(String zoneString);
}

```

任何实现接口`AbstractZoneTimeClient`的类都必须实现方法`getZonedDateTime`；这个方法是一个`abstract`方法，就像接口中的所有其他非默认（非静态）方法一样。

假设您扩展了接口`TimeClient`如下：

```java
public interface HandleInvalidTimeZoneClient extends TimeClient {
    default public ZonedDateTime getZonedDateTime(String zoneString) {
        try {
            return ZonedDateTime.of(getLocalDateTime(),ZoneId.of(zoneString)); 
        } catch (DateTimeException e) {
            System.err.println("Invalid zone ID: " + zoneString +
                "; using the default time zone instead.");
            return ZonedDateTime.of(getLocalDateTime(),ZoneId.systemDefault());
        }
    }
}

```

任何实现接口`HandleInvalidTimeZoneClient`的类都将使用此接口指定的`getZonedDateTime`实现，而不是接口`TimeClient`指定的实现。

## 静态方法

除了默认方法之外，您还可以在接口中定义静态方法。（静态方法是与定义它的类相关联的方法，而不是与任何对象相关联的方法。类的每个实例共享其静态方法。）这使您更容易在库中组织辅助方法；您可以将特定于接口的静态方法保留在同一接口中，而不是在单独的类中。以下示例定义了一个静态方法，用于检索与时区标识符对应的[`ZoneId`](https://docs.oracle.com/javase/8/docs/api/java/time/ZoneId.html)对象；如果没有与给定标识符对应的`ZoneId`对象，则使用系统默认时区。（因此，您可以简化方法`getZonedDateTime`）：

```java
public interface TimeClient {
    // ...
    static public ZoneId getZoneId (String zoneString) {
        try {
            return ZoneId.of(zoneString);
        } catch (DateTimeException e) {
            System.err.println("Invalid time zone: " + zoneString +
                "; using default time zone instead.");
            return ZoneId.systemDefault();
        }
    }

    default public ZonedDateTime getZonedDateTime(String zoneString) {
        return ZonedDateTime.of(getLocalDateTime(), getZoneId(zoneString));
    }    
}

```

就像类中的静态方法一样，您可以在接口中的方法定义之前使用`static`关键字指定一个方法是静态方法。接口中的所有方法声明，包括静态方法，都隐式为`public`，因此您可以省略`public`修饰符。

## 将默认方法集成到现有库中

默认方法使您可以向现有接口添加新功能，并确保与为旧版本接口编写的代码具有二进制兼容性。特别是，默认方法使您可以向现有接口添加接受 lambda 表达式作为参数的方法。本节演示了如何通过默认方法和静态方法增强了[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)接口。

将`Card`和`Deck`类视为问题和练习：类中描述的那样。此示例将`Card`和`Deck`类重写为接口。`Card`接口包含两个`enum`类型（`Suit`和`Rank`）和两个抽象方法（`getSuit`和`getRank`）：

```java

package defaultmethods;

public interface Card extends Comparable<Card> {

    public enum Suit { 
        DIAMONDS (1, "Diamonds"), 
        CLUBS    (2, "Clubs"   ), 
        HEARTS   (3, "Hearts"  ), 
        SPADES   (4, "Spades"  );

        private final int value;
        private final String text;
        Suit(int value, String text) {
            this.value = value;
            this.text = text;
        }
        public int value() {return value;}
        public String text() {return text;}
    }

    public enum Rank { 
        DEUCE  (2 , "Two"  ),
        THREE  (3 , "Three"), 
        FOUR   (4 , "Four" ), 
        FIVE   (5 , "Five" ), 
        SIX    (6 , "Six"  ), 
        SEVEN  (7 , "Seven"),
        EIGHT  (8 , "Eight"), 
        NINE   (9 , "Nine" ), 
        TEN    (10, "Ten"  ), 
        JACK   (11, "Jack" ),
        QUEEN  (12, "Queen"), 
        KING   (13, "King" ),
        ACE    (14, "Ace"  );
        private final int value;
        private final String text;
        Rank(int value, String text) {
            this.value = value;
            this.text = text;
        }
        public int value() {return value;}
        public String text() {return text;}
    }

    public Card.Suit getSuit();
    public Card.Rank getRank();
}

```

`Deck`接口包含各种操作牌组中卡片的方法：

```java

package defaultmethods; 

import java.util.*;
import java.util.stream.*;
import java.lang.*;

public interface Deck {

    List<Card> getCards();
    Deck deckFactory();
    int size();
    void addCard(Card card);
    void addCards(List<Card> cards);
    void addDeck(Deck deck);
    void shuffle();
    void sort();
    void sort(Comparator<Card> c);
    String deckToString();

    Map<Integer, Deck> deal(int players, int numberOfCards)
        throws IllegalArgumentException;

}

```

类`PlayingCard`实现了接口`Card`，而类`StandardDeck`实现了接口`Deck`。

类`StandardDeck`按如下方式实现了抽象方法`Deck.sort`：

```java
public class StandardDeck implements Deck {

    private List<Card> entireDeck;

    // ...

    public void sort() {
        Collections.sort(entireDeck);
    }

    // ...
}

```

方法`Collections.sort`对实现接口[`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)的元素类型为`List`的实例进行排序。成员`entireDeck`是一个`List`的实例，其元素类型为扩展了`Comparable`的`Card`类型。类`PlayingCard`按如下方式实现了[`Comparable.compareTo`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#compareTo-T-)方法：

```java
public int hashCode() {
    return ((suit.value()-1)*13)+rank.value();
}

public int compareTo(Card o) {
    return this.hashCode() - o.hashCode();
}

```

方法`compareTo`导致方法`StandardDeck.sort()`首先按花色，然后按等级对牌组进行排序。

如果你想先按等级，然后按花色对牌组进行排序怎么办？你需要实现[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)接口来指定新的排序标准，并使用方法[`sort(List<T> list, Comparator<? super T> c)`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#sort-java.util.List-java.util.Comparator-)（包含`Comparator`参数的`sort`方法版本）。你可以在类`StandardDeck`中定义以下方法：

```java
public void sort(Comparator<Card> c) {
    Collections.sort(entireDeck, c);
}  

```

有了这个方法，你可以指定方法`Collections.sort`如何对`Card`类的实例进行排序。一种方法是实现`Comparator`接口来指定你希望如何对牌进行排序。示例`SortByRankThenSuit`就是这样做的：

```java

package defaultmethods;

import java.util.*;
import java.util.stream.*;
import java.lang.*;

public class SortByRankThenSuit implements Comparator<Card> {
    public int compare(Card firstCard, Card secondCard) {
        int compVal =
            firstCard.getRank().value() - secondCard.getRank().value();
        if (compVal != 0)
            return compVal;
        else
            return firstCard.getSuit().value() - secondCard.getSuit().value(); 
    }
}

```

以下调用首先按等级，然后按花色对扑克牌组进行排序：

```java
StandardDeck myDeck = new StandardDeck();
myDeck.shuffle();
myDeck.sort(new SortByRankThenSuit());

```

然而，这种方法太啰嗦了；如果你可以只指定排序标准而避免创建多个排序实现，那将更好。假设你是编写`Comparator`接口的开发人员。你可以向`Comparator`接口添加哪些默认或静态方法，以使其他开发人员更容易指定排序标准？

首先，假设你想按等级对扑克牌组进行排序，而不考虑花色。你可以如下调用`StandardDeck.sort`方法：

```java
StandardDeck myDeck = new StandardDeck();
myDeck.shuffle();
myDeck.sort(
    (firstCard, secondCard) ->
        firstCard.getRank().value() - secondCard.getRank().value()
); 

```

因为`Comparator`接口是一个函数式接口，您可以使用 lambda 表达式作为`sort`方法的参数。在这个例子中，lambda 表达式比较两个整数值。

如果您的开发人员只需调用方法`Card.getRank`就能创建一个`Comparator`实例，那将更简单。特别是，如果您的开发人员可以创建一个比较任何可以从`getValue`或`hashCode`等方法返回数值的对象的`Comparator`实例，那将很有帮助。`Comparator`接口已经通过静态方法[`comparing`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#comparing-java.util.function.Function-java.util.Comparator-)增强了这种能力：

```java
myDeck.sort(Comparator.comparing((card) -> card.getRank()));  

```

在这个例子中，您可以使用方法引用：

```java
myDeck.sort(Comparator.comparing(Card::getRank));  

```

这种调用更好地演示了如何指定不同的排序标准并避免创建多个排序实现。

`Comparator`接口已经通过其他版本的静态方法`comparing`（如[`comparingDouble`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#comparingDouble-java.util.function.ToDoubleFunction-java.util.Comparator-)和[`comparingLong`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#comparingLong-java.util.function.ToLongFunction-)）进行了增强，使您能够创建比较其他数据类型的`Comparator`实例。

假设您的开发人员想要创建一个可以根据多个标准比较对象的`Comparator`实例。例如，如何先按等级，然后按花色对扑克牌进行排序？与以前一样，您可以使用 lambda 表达式来指定这些排序标准：

```java
StandardDeck myDeck = new StandardDeck();
myDeck.shuffle();
myDeck.sort(
    (firstCard, secondCard) -> {
        int compare =
            firstCard.getRank().value() - secondCard.getRank().value();
        if (compare != 0)
            return compare;
        else
            return firstCard.getSuit().value() - secondCard.getSuit().value();
    }      
); 

```

如果您的开发人员可以从一系列`Comparator`实例构建一个`Comparator`实例，那将更简单。`Comparator`接口已经通过默认方法[`thenComparing`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#thenComparing-java.util.Comparator-)增强了这种能力：

```java
myDeck.sort(
    Comparator
        .comparing(Card::getRank)
        .thenComparing(Comparator.comparing(Card::getSuit)));

```

`Comparator`接口已经通过其他版本的默认方法`thenComparing`（如[`thenComparingDouble`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#thenComparingDouble-java.util.function.ToDoubleFunction-)和[`thenComparingLong`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#thenComparingLong-java.util.function.ToLongFunction-)）进行了增强，使您能够构建比较其他数据类型的`Comparator`实例。

假设您的开发人员想要创建一个`Comparator`实例，使他们能够以相反的顺序对对象集合进行排序。例如，如何按照牌面从大到小的顺序对扑克牌进行排序，从 A 到 2（而不是从 2 到 A）？与以前一样，您可以指定另一个 lambda 表达式。但是，如果开发人员可以通过调用方法来反转现有的`Comparator`，那将更简单。`Comparator`接口已经通过默认方法[`reversed`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html#reversed--)增强了这种能力：

```java
myDeck.sort(
    Comparator.comparing(Card::getRank)
        .reversed()
        .thenComparing(Comparator.comparing(Card::getSuit)));

```

这个例子演示了如何通过默认方法、静态方法、lambda 表达式和方法引用增强了`Comparator`接口，以创建更具表现力的库方法，程序员可以通过查看它们的调用方式快速推断出其功能。使用这些构造来增强您的库中的接口。
