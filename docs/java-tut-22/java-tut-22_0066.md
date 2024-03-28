# Lambda 表达式

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)

匿名类的一个问题是，如果您的匿名类的实现非常简单，例如只包含一个方法的接口，那么匿名类的语法可能显得笨拙和不清晰。在这些情况下，通常您试图将功能作为参数传递给另一个方法，例如当某人单击按钮时应执行什么操作。Lambda 表达式使您能够做到这一点，将功能视为方法参数，或将代码视为数据。

前一节，匿名类，向您展示了如何实现一个没有名称的基类。尽管这通常比具有名称的类更简洁，但对于只有一个方法的类来说，即使是匿名类似乎也有点过多和繁琐。Lambda 表达式让您更简洁地表达单方法类的实例。

本节涵盖以下主题：

+   Lambda 表达式的理想使用情况

    +   方法 1：创建搜索符合一个特征的成员的方法

    +   方法 2：创建更通用的搜索方法

    +   方法 3：在本地类中指定搜索条件代码

    +   方法 4：在匿名类中指定搜索条件代码

    +   方法 5：使用 Lambda 表达式指定搜索条件代码

    +   方法 6：使用 Lambda 表达式与标准功能接口

    +   方法 7：在整个应用程序中使用 Lambda 表达式

    +   方法 8：更广泛地使用泛型

    +   方法 9：使用接受 Lambda 表达式作为参数的聚合操作

+   GUI 应用程序中的 Lambda 表达式

+   Lambda 表达式的语法

+   访问封闭范围的局部变量

+   目标类型

    +   目标类型和方法参数

+   序列化

## Lambda 表达式的理想使用情况

假设您正在创建一个社交网络应用程序。您希望创建一个功能，使管理员能够对满足特定条件的社交网络应用程序成员执行任何类型的操作，例如发送消息。以下表格详细描述了这种用例：

| 字段 | 描述 |
| --- | --- |
| 名称 | 对所选成员执行操作 |
| 主要执行者 | 管理员 |
| 前提条件 | 管理员已登录到系统。 |
| 后置条件 | 操作仅在符合指定条件的成员上执行。 |
| 主要成功场景 |

1.  管理员指定要执行某个操作的成员的条件。

1.  管理员指定对所选成员执行的操作。

1.  管理员选择**提交**按钮。

1.  系统找到所有符合指定条件的成员。

1.  系统对所有匹配成员执行指定操作。

|

| 扩展 | 1a. 管理员在指定执行操作或选择**提交**按钮之前有选项预览符合指定条件的成员。 |
| --- | --- |
| 出现频率 | 一天中多次。 |

假设这个社交网络应用程序的成员由以下`Person`类表示：

```java
public class Person {

    public enum Sex {
        MALE, FEMALE
    }

    String name;
    LocalDate birthday;
    Sex gender;
    String emailAddress;

    public int getAge() {
        // ...
    }

    public void printPerson() {
        // ...
    }
}
```

假设您的社交网络应用程序的成员存储在`List<Person>`实例中。

本节从一个简单的方法开始处理这种用例。它通过本地和匿名类改进了这种方法，然后以使用 lambda 表达式的高效简洁方法结束。在示例`RosterTest`中找到本节描述的代码摘录。

### 方法 1：创建搜索符合一个特征的成员的方法

一个简单的方法是创建几种方法；每种方法搜索符合一个特征的成员，例如性别或年龄。以下方法打印比指定年龄更老的成员：

```java
public static void printPersonsOlderThan(List<Person> roster, int age) {
    for (Person p : roster) {
        if (p.getAge() >= age) {
            p.printPerson();
        }
    }
}
```

**注意**：[`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)是一个有序的[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)。*集合*是将多个元素组合成单个单元的对象。集合用于存储、检索、操作和传递聚合数据。有关集合的更多信息，请参阅 Collections 教程。

这种方法可能会使您的应用程序变得*脆弱*，这是应用程序由于引入更新（如新数据类型）而无法工作的可能性。假设您升级了应用程序并更改了`Person`类的结构，使其包含不同的成员变量；也许该类使用不同的数据类型或算法记录和测量年龄。您将不得不重写大量 API 以适应这种变化。此外，这种方法是不必要地限制性的；例如，如果您想打印比某个年龄更年轻的成员会怎样？

### 方法 2：创建更通用的搜索方法

以下方法比`printPersonsOlderThan`更通用；它打印指定年龄范围内的成员：

```java
public static void printPersonsWithinAgeRange(
    List<Person> roster, int low, int high) {
    for (Person p : roster) {
        if (low <= p.getAge() && p.getAge() < high) {
            p.printPerson();
        }
    }
}
```

如果您想打印指定性别的成员，或者指定性别和年龄范围的组合会怎样？如果您决定更改`Person`类并添加其他属性，例如关系状态或地理位置会怎样？尽管这种方法比`printPersonsOlderThan`更通用，但尝试为每个可能的搜索查询创建单独的方法仍可能导致脆弱的代码。您可以将指定要搜索的条件的代码与不同类分开。

### 方法 3：在本地类中指定搜索条件代码

以下方法打印符合您指定搜索条件的成员：

```java
public static void printPersons(
    List<Person> roster, CheckPerson tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
}
```

此方法检查`roster`参数中包含的每个`Person`实例是否满足`CheckPerson`参数`tester`中指定的搜索条件，方法是调用`tester.test`方法。如果`tester.test`方法返回`true`值，则在`Person`实例上调用`printPersons`方法。

要指定搜索条件，您需要实现`CheckPerson`接口：

```java
interface CheckPerson {
    boolean test(Person p);
}
```

以下类通过为`test`方法指定实现来实现`CheckPerson`接口。该方法过滤符合美国选择性服务资格的成员：如果其`Person`参数是男性且年龄在 18 至 25 岁之间，则返回`true`值：

```java
class CheckPersonEligibleForSelectiveService implements CheckPerson {
    public boolean test(Person p) {
        return p.gender == Person.Sex.MALE &&
            p.getAge() >= 18 &&
            p.getAge() <= 25;
    }
}
```

要使用此类，您需要创建一个新实例并调用`printPersons`方法：

```java
printPersons(
    roster, new CheckPersonEligibleForSelectiveService());
```

尽管这种方法不太脆弱——如果更改`Person`的结构，您不必重新编写方法——但仍然会有额外的代码：为应用程序中计划执行的每个搜索创建一个新接口和一个本地类。由于`CheckPersonEligibleForSelectiveService`实现了一个接口，您可以使用匿名类代替本地类，避免为每个搜索声明一个新类的需要。

### 方法 4：在匿名类中指定搜索条件代码

下面方法`printPersons`的一个参数是一个匿名类，用于过滤符合美国选择性服务资格的成员：即男性且年龄在 18 至 25 岁之间的成员：

```java
printPersons(
    roster,
    new CheckPerson() {
        public boolean test(Person p) {
            return p.getGender() == Person.Sex.MALE
                && p.getAge() >= 18
                && p.getAge() <= 25;
        }
    }
);
```

这种方法减少了所需的代码量，因为您不必为要执行的每个搜索创建一个新类。然而，考虑到`CheckPerson`接口仅包含一个方法，匿名类的语法很臃肿。在这种情况下，您可以使用 Lambda 表达式代替匿名类，如下一节所述。

### 方法 5：使用 Lambda 表达式指定搜索条件代码

`CheckPerson`接口是一个*函数式接口*。函数式接口是仅包含一个抽象方法的任何接口。（函数式接口可以包含一个或多个默认方法或静态方法。）因为函数式接口仅包含一个抽象方法，所以在实现它时可以省略该方法的名称。为此，您可以使用 Lambda 表达式，如下面方法调用中所示：

```java
printPersons(
    roster,
    (Person p) -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25
);
```

有关如何定义 Lambda 表达式的语法，请参阅 Lambda 表达式的语法。

您可以使用标准的函数式接口来替代`CheckPerson`接口，从而进一步减少所需的代码量。

### 第六种方法：使用 Lambda 表达式与标准函数式接口

重新考虑`CheckPerson`接口：

```java
interface CheckPerson {
    boolean test(Person p);
}
```

这是一个非常简单的接口。它是一个函数式接口，因为它只包含一个抽象方法。这个方法接受一个参数并返回一个`boolean`值。这个方法如此简单，以至于在你的应用程序中定义一个可能不值得。因此，JDK 定义了几个标准的函数式接口，你可以在`java.util.function`包中找到。

例如，你可以在`CheckPerson`的位置使用`Predicate<T>`接口。这个接口包含方法`boolean test(T t)`：

```java
interface Predicate<T> {
    boolean test(T t);
}
```

接口`Predicate<T>`是一个泛型接口的示例。（有关泛型的更多信息，请参阅泛型（更新）课程。）泛型类型（如泛型接口）在尖括号（`<>`）内指定一个或多个类型参数。这个接口只包含一个类型参数`T`。当你声明或实例化一个带有实际类型参数的泛型类型时，你就有了一个参数化类型。例如，参数化类型`Predicate<Person>`如下所示：

```java
interface Predicate<Person> {
    boolean test(Person t);
}
```

这个参数化类型包含一个与`CheckPerson.boolean test(Person p)`具有相同返回类型和参数的方法。因此，你可以像下面的方法演示的那样使用`Predicate<T>`来替代`CheckPerson`：

```java
public static void printPersonsWithPredicate(
    List<Person> roster, Predicate<Person> tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
}
```

因此，下面的方法调用与你在第 3 种方法：在本地类中指定搜索条件代码中调用`printPersons`以获取符合选择性服务资格的成员时是相同的：

```java
printPersonsWithPredicate(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25
);
```

这个方法中使用 Lambda 表达式的地方并不是唯一的。以下方法建议其他使用 Lambda 表达式的方式。

### 第七种方法：在整个应用程序中使用 Lambda 表达式

重新考虑`printPersonsWithPredicate`方法，看看还能在哪里使用 Lambda 表达式：

```java
public static void printPersonsWithPredicate(
    List<Person> roster, Predicate<Person> tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
}
```

这个方法检查`roster`参数中包含的每个`Person`实例是否满足`tester`参数指定的条件。如果`Person`实例确实满足`tester`指定的条件，则在`Person`实例上调用`printPerson`方法。

你可以指定一个不同的操作来执行那些满足`tester`指定的条件的`Person`实例，而不是调用`printPerson`方法。你可以用 lambda 表达式指定这个操作。假设你想要一个类似于`printPerson`的 lambda 表达式，一个接受一个参数（一个`Person`类型的对象）并返回`void`的。记住，要使用 lambda 表达式，你需要实现一个函数式接口。在这种情况下，你需要一个包含可以接受一个`Person`类型参数并返回`void`的抽象方法的函数式接口。`Consumer<T>`接口包含方法`void accept(T t)`，具有这些特征。以下方法用一个调用`accept`方法的`Consumer<Person>`实例替换了`p.printPerson()`的调用：

```java
public static void processPersons(
    List<Person> roster,
    Predicate<Person> tester,
    Consumer<Person> block) {
        for (Person p : roster) {
            if (tester.test(p)) {
                block.accept(p);
            }
        }
}
```

因此，以下方法调用与在方法 3：在本地类中指定搜索条件代码中调用`printPersons`以获取符合应征条件的成员时是相同的。用于打印成员的 lambda 表达式被突出显示：

```java
processPersons(
     roster,
     p -> p.getGender() == Person.Sex.MALE
         && p.getAge() >= 18
         && p.getAge() <= 25,
     p -> p.printPerson()
);
```

如果你想对成员的个人资料做更多操作而不仅仅是打印它们。假设你想验证成员的个人资料或检索他们的联系信息？在这种情况下，你需要一个包含返回值的抽象方法的函数式接口。`Function<T,R>`接口包含方法`R apply(T t)`。以下方法检索由参数`mapper`指定的数据，然后执行由参数`block`指定的操作：

```java
public static void processPersonsWithFunction(
    List<Person> roster,
    Predicate<Person> tester,
    Function<Person, String> mapper,
    Consumer<String> block) {
    for (Person p : roster) {
        if (tester.test(p)) {
            String data = mapper.apply(p);
            block.accept(data);
        }
    }
}
```

以下方法从`roster`中包含的每个符合应征条件的成员中检索电子邮件地址，然后打印它：

```java
processPersonsWithFunction(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);
```

### 方法 8：更广泛地使用泛型

重新考虑`processPersonsWithFunction`方法。以下是一个通用版本，它接受一个包含任何数据类型元素的集合作为参数：

```java
public static <X, Y> void processElements(
    Iterable<X> source,
    Predicate<X> tester,
    Function <X, Y> mapper,
    Consumer<Y> block) {
    for (X p : source) {
        if (tester.test(p)) {
            Y data = mapper.apply(p);
            block.accept(data);
        }
    }
}
```

要打印符合应征条件的成员的电子邮件地址，请按照以下方式调用`processElements`方法：

```java
processElements(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);
```

此方法调用执行以下操作：

1.  从集合`source`中获取对象的源。在这个例子中，它从集合`roster`中获取`Person`对象的源。注意，集合`roster`是一个`List`类型的集合，也是一个`Iterable`类型的对象。

1.  过滤与`tester`对象匹配的对象。在这个例子中，`Predicate`对象是一个指定哪些成员符合应征条件的 lambda 表达式。

1.  将每个经过筛选的对象映射到由`mapper`对象指定的值。在这个例子中，`Function`对象是一个返回成员电子邮件地址的 lambda 表达式。

1.  根据`Consumer`对象`block`指定的操作对每个映射对象执行动作。在此示例中，`Consumer`对象是一个打印字符串的 Lambda 表达式，该字符串是由`Function`对象返回的电子邮件地址。

您可以用聚合操作替换每个这些操作。

### 方法 9：使用接受 Lambda 表达式作为参数的聚合操作

以下示例使用聚合操作打印出集合`roster`中符合选择性服务资格的成员的电子邮件地址：

```java
roster
    .stream()
    .filter(
        p -> p.getGender() == Person.Sex.MALE
            && p.getAge() >= 18
            && p.getAge() <= 25)
    .map(p -> p.getEmailAddress())
    .forEach(email -> System.out.println(email));
```

以下表格将方法`processElements`执行的每个操作与相应的聚合操作进行了映射：

| `processElements`操作 | 聚合操作 |
| --- | --- |
| 获取对象源 | `Stream<E> **stream**()` |
| 过滤与`Predicate`对象匹配的对象 | `Stream<T> **filter**(Predicate<? super T> predicate)` |
| 根据`Function`对象将对象映射到另一个值 | `<R> Stream<R> **map**(Function<? super T,? extends R> mapper)` |
| 根据`Consumer`对象指定的操作执行动作 | `void **forEach**(Consumer<? super T> action)` |

操作`filter`、`map`和`forEach`是*聚合操作*。聚合操作处理来自流的元素，而不是直接来自集合（这就是为什么此示例中调用的第一个方法是`stream`的原因）。*流*是元素的序列。与集合不同，它不是存储元素的数据结构。相反，流通过管道从源（例如集合）传递值。*管道*是一系列流操作，本示例中是`filter`-`map`-`forEach`。此外，聚合操作通常接受 Lambda 表达式作为参数，使您能够自定义它们的行为。

对于更深入讨论聚合操作，请参阅聚合操作课程。

## GUI 应用程序中的 Lambda 表达式

要处理图形用户界面（GUI）应用程序中的事件，例如键盘操作、鼠标操作和滚动操作，通常需要创建事件处理程序，这通常涉及实现特定的接口。通常，事件处理程序接口是函数式接口；它们往往只有一个方法。

在 JavaFX 示例[`HelloWorld.java`](https://docs.oracle.com/javase/8/javafx/get-started-tutorial/hello_world.htm)（在上一节匿名类中讨论）中，您可以在此语句中用 Lambda 表达式替换突出显示的匿名类：

```java
        btn.setOnAction(new EventHandler<ActionEvent>() {

            @Override
            public void handle(ActionEvent event) {
                System.out.println("Hello World!");
            }
        });
```

方法调用`btn.setOnAction`指定了当选择由`btn`对象表示的按钮时会发生什么。此方法需要一个`EventHandler<ActionEvent>`类型的对象。`EventHandler<ActionEvent>`接口只包含一个方法`void handle(T event)`。该接口是一个函数式接口，因此您可以使用以下突出显示的 Lambda 表达式来替换它：

```java
        btn.setOnAction(
          event -> System.out.println("Hello World!")
        );
```

## Lambda 表达式的语法

一个 lambda 表达式由以下内容组成：

+   用括号括起的逗号分隔的形式参数列表。`CheckPerson.test`方法包含一个参数`p`，它表示`Person`类的一个实例。

    **注意**：您可以在 lambda 表达式中省略参数的数据类型。此外，如果只有一个参数，您可以省略括号。例如，以下 lambda 表达式也是有效的：

    ```java
    p -> p.getGender() == Person.Sex.MALE 
        && p.getAge() >= 18
        && p.getAge() <= 25
    ```

+   箭头标记，`->`

+   一个由单个表达式或语句块组成的主体。本示例使用以下表达式：

    ```java
    p.getGender() == Person.Sex.MALE 
        && p.getAge() >= 18
        && p.getAge() <= 25
    ```

    如果您指定一个单一表达式，那么 Java 运行时将评估该表达式，然后返回其值。或者，您可以使用一个返回语句：

    ```java
    p -> {
        return p.getGender() == Person.Sex.MALE
            && p.getAge() >= 18
            && p.getAge() <= 25;
    }
    ```

    返回语句不是一个表达式；在 lambda 表达式中，您必须用大括号（`{}`）括起语句。然而，在 void 方法调用中，您不必用大括号括起。例如，以下是一个有效的 lambda 表达式：

    ```java
    email -> System.out.println(email)
    ```

请注意，lambda 表达式看起来很像方法声明；您可以将 lambda 表达式视为匿名方法——没有名称的方法。

以下示例`Calculator`是一个使用多个形式参数的 lambda 表达式的示例：

```java

public class Calculator {

    interface IntegerMath {
        int operation(int a, int b);   
    }

    public int operateBinary(int a, int b, IntegerMath op) {
        return op.operation(a, b);
    }

    public static void main(String... args) {

        Calculator myApp = new Calculator();
        IntegerMath addition = (a, b) -> a + b;
        IntegerMath subtraction = (a, b) -> a - b;
        System.out.println("40 + 2 = " +
            myApp.operateBinary(40, 2, addition));
        System.out.println("20 - 10 = " +
            myApp.operateBinary(20, 10, subtraction));    
    }
}

```

方法`operateBinary`对两个整数操作数执行数学运算。操作本身由`IntegerMath`的实例指定。该示例使用 lambda 表达式定义了两个操作，`addition`和`subtraction`。该示例打印如下内容：

```java
40 + 2 = 42
20 - 10 = 10
```

## 访问封闭范围的局部变量

像局部类和匿名类一样，lambda 表达式可以捕获变量；它们对封闭范围的局部变量具有相同的访问权限。然而，与局部类和匿名类不同，lambda 表达式没有任何遮蔽问题（有关更多信息，请参见遮蔽）。Lambda 表达式是词法作用域的。这意味着它们不继承任何名称来自超类型，也不引入新的作用域级别。lambda 表达式中的声明被解释为在封闭环境中一样。以下示例`LambdaScopeTest`演示了这一点：

```java

import java.util.function.Consumer;

public class LambdaScopeTest {

    public int x = 0;

    class FirstLevel {

        public int x = 1;

        void methodInFirstLevel(int x) {

            int z = 2;

            Consumer<Integer> myConsumer = (y) -> 
            {
                // The following statement causes the compiler to generate
                // the error "Local variable z defined in an enclosing scope
                // must be final or effectively final" 
                //
                // z = 99;

                System.out.println("x = " + x); 
                System.out.println("y = " + y);
                System.out.println("z = " + z);
                System.out.println("this.x = " + this.x);
                System.out.println("LambdaScopeTest.this.x = " +
                    LambdaScopeTest.this.x);
            };

            myConsumer.accept(x);

        }
    }

    public static void main(String... args) {
        LambdaScopeTest st = new LambdaScopeTest();
        LambdaScopeTest.FirstLevel fl = st.new FirstLevel();
        fl.methodInFirstLevel(23);
    }
}
```

本示例生成以下输出：

```java
x = 23
y = 23
z = 2
this.x = 1
LambdaScopeTest.this.x = 0

```

如果在 lambda 表达式`myConsumer`的声明中，将参数`x`替换为`y`，那么编译器会生成一个错误：

```java
Consumer<Integer> myConsumer = (x) -> {
    // ...
}
```

编译器生成错误“Lambda 表达式的参数 x 不能重新声明在封闭范围中定义的另一个局部变量”，因为 lambda 表达式不引入新的作用域级别。因此，可以直接访问封闭范围的字段、方法和局部变量。例如，lambda 表达式直接访问方法`methodInFirstLevel`的参数`x`。要访问封闭类中的变量，请使用关键字`this`。在这个例子中，`this.x`指的是成员变量`FirstLevel.x`。

然而，与本地和匿名类一样，lambda 表达式只能访问封闭块的局部变量和参数，这些变量必须是 final 或有效 final。在这个例子中，变量`z`是有效 final；在初始化后其值不会改变。然而，假设在 lambda 表达式`myConsumer`中添加以下赋值语句：

```java
Consumer<Integer> myConsumer = (y) -> {
    z = 99;
    // ...
}
```

由于这个赋值语句，变量`z`不再是有效 final。因此，Java 编译器生成类似于“定义在封闭范围中的局部变量 z 必须是 final 或有效 final”的错误消息。

## 目标类型

如何确定 lambda 表达式的类型？回想一下选择男性会员且年龄在 18 到 25 岁之间的 lambda 表达式：

```java
p -> p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25
```

此 lambda 表达式在以下两个方法中使用：

+   方法 3：在本地类中指定搜索条件代码 中的 `public static void printPersons(List<Person> roster, CheckPerson tester)`

+   方法 6：使用标准函数接口和 Lambda 表达式 中的 `public void printPersonsWithPredicate(List<Person> roster, Predicate<Person> tester)`

当 Java 运行时调用方法 `printPersons` 时，它期望的数据类型是 `CheckPerson`，因此 lambda 表达式就是这种类型。然而，当 Java 运行时调用方法 `printPersonsWithPredicate` 时，它期望的数据类型是 `Predicate<Person>`，因此 lambda 表达式就是这种类型。这些方法期望的数据类型称为*目标类型*。为了确定 lambda 表达式的类型，Java 编译器使用 lambda 表达式所在上下文或情况的目标类型。由此可知，只能在 Java 编译器能够确定目标类型的情况下使用 lambda 表达式：

+   变量声明

+   赋值语句

+   返回语句

+   数组初始化器

+   方法或构造函数参数

+   Lambda 表达式主体

+   条件表达式，`?:`

+   强制类型转换表达式

### 目标类型和方法参数

对于方法参数，Java 编译器使用另外两个语言特性来确定目标类型：重载解析和类型参数推断。

考虑以下两个函数式接口（[`java.lang.Runnable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html)和[`java.util.concurrent.Callable<V>`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Callable.html)）：

```java
public interface Runnable {
    void run();
}

public interface Callable<V> {
    V call();
}
```

方法`Runnable.run`不返回值，而`Callable<V>.call`返回值。

假设您已经重载了方法`invoke`如下（有关重载方法的更多信息，请参见定义方法）：

```java
void invoke(Runnable r) {
    r.run();
}

<T> T invoke(Callable<T> c) {
    return c.call();
}
```

在下面的语句中将调用哪个方法？

```java
String s = invoke(() -> "done");
```

将调用方法`invoke(Callable<T>)`，因为该方法返回一个值；方法`invoke(Runnable)`不返回值。在这种情况下，lambda 表达式`() -> "done"`的类型是`Callable<T>`。

## 序列化

如果 lambda 表达式的目标类型和捕获的参数都是可序列化的，则可以对其进行序列化。然而，与内部类一样，强烈不建议序列化 lambda 表达式。
