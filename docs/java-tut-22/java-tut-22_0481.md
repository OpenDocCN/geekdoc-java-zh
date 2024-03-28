# 编写事件监听器的一般信息

> 当设计程序时，您可能希望在一个不是公共的类中实现您的事件监听器，而是在更隐蔽的地方。私有实现是更安全的实现。

本节讨论了在应用程序中实现事件处理程序时要牢记的几个设计考虑。然后我们向您介绍了事件对象，这些小对象描述了每个事件。特别是，我们谈到了低级事件和语义事件的概念，建议您在可能的情况下优先选择语义事件。本节的其余部分讨论了您可能在一些事件监听器中使用的实现技术，或者在其他人或 GUI 构建器创建的事件监听器中看到的技术。

+   设计考虑

+   获取事件信息：事件对象

+   概念：低级事件和语义事件

+   事件适配器

+   获取事件信息：事件对象

+   原文：[`docs.oracle.com/javase/tutorial/uiswing/events/generalrules.html`](https://docs.oracle.com/javase/tutorial/uiswing/events/generalrules.html)

## 设计考虑

关于事件监听器最重要的规则是它们应该执行得非常快速。因为所有绘图和事件监听方法都在同一个线程中执行，一个慢速的事件监听器方法会使程序看起来无响应且重绘速度缓慢。如果需要作为事件结果执行一些耗时操作，请通过启动另一个线程（或以某种方式发送请求到另一个线程）来执行操作。有关使用线程的帮助，请参阅 Swing 中的并发性。

您有许多选择来实现事件监听器。我们无法推荐特定的方法，因为一个解决方案并不适用于所有情况。但是，我们可以给您一些提示，并展示一些您可能看到的技术，即使您在程序中没有使用相同的解决方案。

例如，您可能选择为不同类型的事件监听器实现单独的类。这种架构易于维护，但许多类也可能意味着性能降低。

内部类和匿名内部类

如果您有一种非常特定的简单事件监听器，您可能可以通过使用`EventHandler`类而根本不创建类来避免。

## EventHandler 类

每个事件监听器方法都有一个参数，即从[`EventObject`](https://docs.oracle.com/javase/8/docs/api/java/util/EventObject.html)类继承的对象。尽管参数总是继承自`EventObject`，但其类型通常更加精确地指定。例如，处理鼠标事件的方法的参数是`MouseEvent`的实例，其中`MouseEvent`是`EventObject`的间接子类。

`EventObject` 类定义了一个非常有用的方法：

**`Object getSource()`**

返回触发事件的对象。

注意，`getSource` 方法返回一个 `Object`。事件类有时定义类似于 `getSource` 的方法，但返回类型更受限制。例如，`ComponentEvent` 类定义了一个 `getComponent` 方法，就像 `getSource` 一样返回触发事件的对象。不同之处在于 `getComponent` 总是返回一个 `Component`。每个事件监听器的操作页面都会提到你应该使用 `getSource` 还是其他方法来获取事件源。

通常，事件类定义了返回有关事件信息的方法。例如，你可以查询 `MouseEvent` 对象以获取有关事件发生位置、用户点击次数、按下的修改键等信息。

## 概念：低级事件和语义事件

事件可以分为两组：*低级* 事件和 *语义* 事件。低级事件代表窗口系统发生或低级输入。其他一切都是语义事件。

低级事件的示例包括鼠标和键盘事件，这两者都直接由用户输入引起。语义事件的示例包括动作和项目事件。语义事件可能由用户输入触发；例如，当用户点击按钮时，按钮通常会触发动作事件，当用户按下 *Enter* 键时，文本字段会触发动作事件。然而，有些语义事件根本不是由低级事件触发的。例如，当表模型从数据库接收新数据时，可能会触发表模型事件。

尽可能监听语义事件而不是低级事件。这样，你可以使你的代码尽可能健壮和可移植。例如，监听按钮上的动作事件，而不是鼠标事件，意味着当用户尝试使用键盘替代方案或外观特定手势激活按钮时，按钮将做出适当反应。处理复合组件（如组合框）时，必须坚持使用语义事件，因为你没有可靠的方法在所有可能用于形成复合组件的外观特定组件上注册监听器。

## 事件适配器

一些监听器接口包含多个方法。例如，`MouseListener` 接口包含五个方法：`mousePressed`、`mouseReleased`、`mouseEntered`、`mouseExited` 和 `mouseClicked`。即使你只关心鼠标点击，如果你的类直接实现了 `MouseListener`，那么你必须实现所有五个 `MouseListener` 方法。对于你不关心的事件，方法可以留空。这里是一个例子：

```java
//An example that implements a listener interface directly.
public class MyClass implements MouseListener {
    ...
        someObject.addMouseListener(this);
    ...
    /* Empty method definition. */
    public void mousePressed(MouseEvent e) {
    }

    /* Empty method definition. */
    public void mouseReleased(MouseEvent e) {
    }

    /* Empty method definition. */
    public void mouseEntered(MouseEvent e) {
    }

    /* Empty method definition. */
    public void mouseExited(MouseEvent e) {
    }

    public void mouseClicked(MouseEvent e) {
        *...//Event listener implementation goes here...*
    }
}

```

一系列空方法体的结果可能会使代码更难阅读和维护。为了帮助您避免实现空方法体，API 通常为每个具有多个方法的监听器接口包含一个*适配器*类。（Listener API Table 列出了所有监听器及其适配器。）例如，`MouseAdapter`类实现了`MouseListener`接口。适配器类实现了其接口所有方法的空版本。

要使用适配器，您创建一个它的子类并仅重写感兴趣的方法，而不是直接实现监听器接口的所有方法。以下是修改前述代码以扩展`MouseAdapter`的示例。通过扩展`MouseAdapter`，它继承了`MouseListener`包含的所有五个方法的空定义。

```java
/*
 * An example of extending an adapter class instead of
 * directly implementing a listener interface.
 */
public class MyClass extends MouseAdapter {
    ... 
        someObject.addMouseListener(this);
    ... 
    public void mouseClicked(MouseEvent e) {
        *...//Event listener implementation goes here...*
    }
}

```

## 内部类和匿名内部类

如果您想使用适配器类，但不希望您的公共类继承自适配器类怎么办？例如，假设您编写了一个小程序，并且希望您的`Applet`子类包含一些处理鼠标事件的代码。由于 Java 语言不允许多重继承，您的类不能同时扩展`Applet`和`MouseAdapter`类。解决方案是定义一个*内部类*，即在`Applet`子类内部扩展`MouseAdapter`类。

内部类还可以用于直接实现一个或多个接口的事件监听器。

```java
//An example of using an inner class.
public class MyClass extends Applet {
    ...
        someObject.addMouseListener(new MyAdapter());
    ...
    class MyAdapter extends MouseAdapter {
        public void mouseClicked(MouseEvent e) {
            *...//Event listener implementation goes here...*
        }
    }
}

```

* * *

**性能注意：**

在考虑是否使用内部类时，请记住应用程序启动时间和内存占用量通常与加载的类数量成正比。您创建的类越多，程序启动时间越长，占用的内存也越多。作为应用程序开发人员，您必须在其他设计约束条件下平衡这一点。我们并不建议您将应用程序变成一个单一的庞大类，希望缩短启动时间和内存占用量，这将导致不必要的麻烦和维护负担。

* * *

您可以创建一个不指定名称的内部类，这被称为*匿名内部类*。虽然乍一看可能会觉得奇怪，但匿名内部类可以使您的代码更易于阅读，因为类是在引用它的地方定义的。但是，您需要权衡方便性与增加类数量可能带来的性能影响。 

以下是使用匿名内部类的示例：

```java
//An example of using an anonymous inner class.
public class MyClass extends Applet {
    ...
        someObject.addMouseListener(new MouseAdapter() {
            public void mouseClicked(MouseEvent e) {
                *...//Event listener implementation goes here...*
            }
        });
    ...
    }
}

```

* * *

**注意：**

匿名内部类的一个缺点是它们无法被长期持久性机制所看到。有关更多信息，请参阅[JavaBeans™ package](https://docs.oracle.com/javase/8/docs/api/java/beans/package-summary.html#package_description)的 API 文档以及 JavaBeans trail 中的 Bean Persistence 课程。

* * *

内部类即使需要访问封闭类的私有实例变量，也可以正常工作。只要不将内部类声明为`static`，内部类就可以像在包含类中一样引用实例变量和方法。要使局部变量对内部类可用，只需将变量的副本保存为`final`局部变量。

要引用封闭实例，可以使用`*EnclosingClass*.this`。有关内部类的更多信息，请参见 Nested Classes。

## EventHandler 类

[`EventHandler`](https://docs.oracle.com/javase/8/docs/api/java/beans/EventHandler.html)类支持简单、一语句事件监听器的动态生成。虽然`EventHandler`仅对一种极其简单的事件监听器类型有用，但由于两个原因值得一提。它适用于：

+   创建一个事件监听器，持久性可以看到，但不会用事件监听器接口和方法堵塞自己的类。

+   不增加应用程序中定义的类的数量可以提高性能。

手动创建`EventHandler`很困难。`EventHandler`必须小心构建。如果出错，编译时不会通知您，它会在运行时抛出一个晦涩的异常。因此，最好由 GUI 构建器创建`EventHandler`。`EventHandler`应该仔细记录。否则，您可能会产生难以阅读的代码。

`EventHandler`类旨在供交互式工具使用，例如应用程序构建器，允许开发人员在 bean 之间建立连接。通常，连接是从用户界面 bean（事件源）到应用程序逻辑 bean（目标）进行的。这种连接的最有效方式是将应用程序逻辑与用户界面隔离开来。例如，从 JCheckBox 到接受布尔值的方法的连接的`EventHandler`可以处理提取复选框状态并直接传递给方法，使方法与用户界面层隔离开来。

内部类是处理用户界面事件的另一种更通用的方式。`EventHandler`类仅处理使用内部类可能实现的一部分功能。但是，`EventHandler`与长期持久性方案的配合效果比内部类更好。此外，在大型应用程序中，使用`EventHandler`可以减少应用程序的磁盘和内存占用。

使用`EventHandler`的示例`EventHandler`的最简单用法是安装一个调用目标对象上没有参数的方法的监听器。在以下示例中，我们创建一个 ActionListener，调用`javax.swing.JFrame`实例上的`toFront`方法。

```java
    myButton.addActionListener(
        (ActionListener)EventHandler.create(ActionListener.class, frame, "toFront"));

```

当按下`myButton`时，语句`frame.toFront()`将被执行。通过定义`ActionListener`接口的新实现并将其实例添加到按钮中，也可以获得相同的效果，并获得一些额外的编译时类型安全性：

```java
    //Equivalent code using an inner class instead of EventHandler.
    myButton.addActionListener(new ActionListener() {
        public void actionPerformed(ActionEvent e) {
            frame.toFront();
        }
    });

```

下一个最简单的`EventHandler`的用法是从监听器接口的方法的第一个参数（通常是事件对象）中提取一个属性值，并将其用于设置目标对象的属性值。在下面的示例中，我们创建了一个`ActionListener`，将目标对象（myButton）的`nextFocusableComponent`属性设置为事件的`source`属性的值。

```java
    EventHandler.create(ActionListener.class, myButton, "nextFocusableComponent", "source")

```

这对应于以下内部类实现：

```java
    //Equivalent code using an inner class instead of EventHandler.
    new ActionListener() {
        public void actionPerformed(ActionEvent e) {
            myButton.setNextFocusableComponent((Component)e.getSource()); 
        }
    }

```

也可以创建一个仅将传入事件对象传递给目标动作的`EventHandler`。如果第四个`EventHandler.create`参数是一个空字符串，那么事件就会被简单地传递下去：

```java
    EventHandler.create(ActionListener.class, target, "doActionEvent", "")

```

这对应于以下内部类实现：

```java
    //Equivalent code using an inner class instead of EventHandler.
    new ActionListener() {
        public void actionPerformed(ActionEvent e) {
            target.doActionEvent(e);
        }
    }

```

可能最常见的`EventHandler`用法是从事件对象的源中提取一个属性值，并将此值设置为目标对象的属性值。在下面的示例中，我们创建了一个`ActionListener`，将目标对象的`label`属性设置为事件源（事件的`source`属性的值）的`text`属性的值。

```java
    EventHandler.create(ActionListener.class, myButton, "label", "source.text")

```

这对应于以下内部类实现：

```java
    //Equivalent code using an inner class instead of EventHandler.
    new ActionListener {
        public void actionPerformed(ActionEvent e) {
            myButton.setLabel(((JTextField)e.getSource()).getText()); 
        }
    }

```
