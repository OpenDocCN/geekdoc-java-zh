# 覆盖和隐藏方法

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/override.html`](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)

## 实例方法

子类中具有与超类中实例方法相同签名（名称，以及其参数的数量和类型）和返回类型的实例方法*覆盖*了超类的方法。

子类覆盖方法的能力允许一个类从一个行为“足够接近”的超类继承，然后根据需要修改行为。覆盖方法具有与其覆盖的方法相同的名称、参数数量和类型以及返回类型。覆盖方法还可以返回被覆盖方法返回类型的子类型。这个子类型被称为*协变返回类型*。

在覆盖方法时，您可能希望使用`@Override`注解，指示编译器您打算覆盖超类中的方法。如果由于某种原因，编译器检测到该方法在任何一个超类中不存在，则会生成一个错误。有关`@Override`的更多信息，请参见`Annotations`。

## 静态方法

如果一个子类定义了一个与超类中静态方法相同签名的静态方法，则子类中的方法*隐藏*超类中的方法。

隐藏静态方法和覆盖实例方法之间的区别具有重要的影响：

+   调用的覆盖实例方法的版本是在子类中的版本。

+   调用的隐藏静态方法的版本取决于它是从超类还是从子类调用的。

考虑一个包含两个类的示例。第一个是`Animal`，包含一个实例方法和一个静态方法：

```java
public class Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Animal");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Animal");
    }
}

```

第二个类，`Cat`，是`Animal`的一个子类：

```java
public class Cat extends Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Cat");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Cat");
    }

    public static void main(String[] args) {
        Cat myCat = new Cat();
        Animal myAnimal = myCat;
        Animal.testClassMethod();
        myAnimal.testInstanceMethod();
    }
}

```

`Cat`类覆盖了`Animal`中的实例方法，并隐藏了`Animal`中的静态方法。这个类中的`main`方法创建了一个`Cat`的实例，并在类上调用`testClassMethod()`，在实例上调用`testInstanceMethod()`。

这个程序的输出如下：

```java
The static method in Animal
The instance method in Cat

```

如约定，调用的隐藏静态方法的版本是在超类中的版本，调用的覆盖实例方法的版本是在子类中的版本。

## 接口方法

默认方法和抽象方法在接口中像实例方法一样被继承。然而，当一个类或接口的超类型提供了多个具有相同签名的默认方法时，Java 编译器遵循继承规则来解决名称冲突。这些规则受以下两个原则驱动：

+   实例方法优先于接口默认方法。

    考虑以下类和接口：

    ```java
    public class Horse {
        public String identifyMyself() {
            return "I am a horse.";
        }
    }
    ```

    ```java
    public interface Flyer {
        default public String identifyMyself() {
            return "I am able to fly.";
        }
    }
    ```

    ```java
    public interface Mythical {
        default public String identifyMyself() {
            return "I am a mythical creature.";
        }
    }
    ```

    ```java
    public class Pegasus extends Horse implements Flyer, Mythical {
        public static void main(String... args) {
            Pegasus myApp = new Pegasus();
            System.out.println(myApp.identifyMyself());
        }
    }
    ```

    方法`Pegasus.identifyMyself`返回字符串`I am a horse.`

+   已经被其他候选方法覆盖的方法将被忽略。当超类型共享一个共同的祖先时，就会出现这种情况。

    考虑以下接口和类：

    ```java
    public interface Animal {
        default public String identifyMyself() {
            return "I am an animal.";
        }
    }
    ```

    ```java
    public interface EggLayer extends Animal {
        default public String identifyMyself() {
            return "I am able to lay eggs.";
        }
    }
    ```

    ```java
    public interface FireBreather extends Animal { }
    ```

    ```java
    public class Dragon implements EggLayer, FireBreather {
        public static void main (String... args) {
            Dragon myApp = new Dragon();
            System.out.println(myApp.identifyMyself());
        }
    }
    ```

    方法`Dragon.identifyMyself`返回字符串`I am able to lay eggs.`

如果两个或更多独立定义的默认方法冲突，或者默认方法与抽象方法冲突，则 Java 编译器会产生编译错误。您必须显式覆盖超类型方法。

考虑一下关于现在可以飞行的计算机控制汽车的例子。您有两个接口（`OperateCar`和`FlyCar`），它们为相同方法（`startEngine`）提供默认实现：

```java
public interface OperateCar {
    // ...
    default public int startEngine(EncryptedKey key) {
        // Implementation
    }
}
```

```java
public interface FlyCar {
    // ...
    default public int startEngine(EncryptedKey key) {
        // Implementation
    }
}
```

实现`OperateCar`和`FlyCar`的类必须覆盖方法`startEngine`。您可以使用`super`关键字调用任何默认实现中的任何一个。

```java
public class FlyingCar implements OperateCar, FlyCar {
    // ...
    public int startEngine(EncryptedKey key) {
        FlyCar.super.startEngine(key);
        OperateCar.super.startEngine(key);
    }
}
```

在`super`之前的名称（在本例中为`FlyCar`或`OperateCar`）必须引用直接定义或继承了被调用方法的超接口。这种形式的方法调用不仅限于区分包含具有相同签名的默认方法的多个实现接口。您可以使用`super`关键字在类和接口中调用默认方法。

从类中继承的实例方法可以覆盖抽象接口方法。考虑以下接口和类：

```java
public interface Mammal {
    String identifyMyself();
}

```

```java
public class Horse {
    public String identifyMyself() {
        return "I am a horse.";
    }
}

```

```java
public class Mustang extends Horse implements Mammal {
    public static void main(String... args) {
        Mustang myApp = new Mustang();
        System.out.println(myApp.identifyMyself());
    }
}

```

方法`Mustang.identifyMyself`返回字符串`I am a horse.` 类`Mustang`继承自类`Horse`的方法`identifyMyself`，该方法覆盖了接口`Mammal`中同名的抽象方法。

**注意**：接口中的静态方法不会被继承。

## 修饰符

覆盖方法的访问修饰符可以允许更多的访问权限，但不能少于被覆盖方法的访问权限。例如，超类中的受保护实例方法可以在子类中变为公共方法，但不能变为私有方法。

如果您尝试将超类中的实例方法更改为子类中的静态方法，或者反之，则会收到编译时错误。

## 总结

以下表格总结了当您定义一个与超类中方法具有相同签名的方法时会发生什么。

定义一个与超类方法具有相同签名的方法

|   | 超类实例方法 | 超类静态方法 |
| --- | --- | --- |
| 子类实例方法 | 覆盖 | 生成编译时错误 |
| 子类静态方法 | 生成编译时错误 | 隐藏 |

* * *

**注意**：在子类中，您可以重载从超类继承的方法。这样重载的方法既不隐藏也不覆盖超类实例方法——它们是子类独有的新方法。

* * *
