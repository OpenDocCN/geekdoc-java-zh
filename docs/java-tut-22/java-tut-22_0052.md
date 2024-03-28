# 使用对象

> 译文：[`docs.oracle.com/javase/tutorial/java/javaOO/usingobject.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/usingobject.html)

创建对象后，您可能想要对其进行某些操作。您可能需要使用其中一个字段的值，更改其中一个字段，或调用其中一个方法执行操作。

## 引用对象的字段

通过它们的名称访问对象字段。您必须使用一个不含糊的名称。

您可以在其自身类中使用字段的简单名称。例如，我们可以在`Rectangle`类中添加一个语句并打印`width`和`height`：

```java
System.out.println("Width and height are: " + width + ", " + height);

```

在这种情况下，`width`和`height`是简单名称。

在对象的类之外的代码必须使用对象引用或表达式，后跟点（.）运算符，后跟一个简单的字段名称，如：

```java
objectReference.fieldName

```

例如，`CreateObjectDemo`类中的代码位于`Rectangle`类的代码之外。因此，要引用`Rectangle`对象`rectOne`中的`origin`、`width`和`height`字段，`CreateObjectDemo`类必须分别使用名称`rectOne.origin`、`rectOne.width`和`rectOne.height`。程序使用这两个名称来显示`rectOne`的`width`和`height`：

```java
System.out.println("Width of rectOne: "  + rectOne.width);
System.out.println("Height of rectOne: " + rectOne.height);

```

尝试在`CreateObjectDemo`类中的代码中使用简单名称`width`和`height`是没有意义的  这些字段只存在于对象内部  并且会导致编译器错误。

稍后，程序使用类似的代码来显示有关`rectTwo`的信息。相同类型的对象具有自己的相同实例字段的副本。因此，每个`Rectangle`对象都有名为`origin`、`width`和`height`的字段。当您通过对象引用访问实例字段时，您引用特定对象的字段。`CreateObjectDemo`程序中的两个对象`rectOne`和`rectTwo`具有不同的`origin`、`width`和`height`字段。

要访问字段，您可以使用一个命名引用对象，就像前面的例子中那样，或者您可以使用任何返回对象引用的表达式。请记住，`new`运算符返回一个对象的引用。因此，您可以使用从`new`返回的值来访问新对象的字段：

```java
int height = new Rectangle().height;

```

此语句创建一个新的`Rectangle`对象并立即获取其高度。实质上，该语句计算了`Rectangle`的默认高度。请注意，在执行此语句后，程序不再具有对创建的`Rectangle`的引用，因为程序从未将引用存储在任何地方。该对象没有引用，其资源可以被 Java 虚拟机回收。

## 调用对象的方法

您还可以使用对象引用来调用对象的方法。您将方法的简单名称附加到对象引用上，中间使用点运算符（.）。此外，您可以在括号内提供方法的任何参数。如果方法不需要任何参数，请使用空括号。

```java
objectReference.methodName(argumentList);

```

或者：

```java
objectReference.methodName();

```

`Rectangle`类有两个方法：`getArea()`用于计算矩形的面积和`move()`用于改变矩形的原点。这是调用这两个方法的`CreateObjectDemo`代码：

```java
System.out.println("Area of rectOne: " + rectOne.getArea());
...
rectTwo.move(40, 72);

```

第一条语句调用`rectOne`的`getArea()`方法并显示结果。第二行移动`rectTwo`，因为`move()`方法为对象的`origin.x`和`origin.y`赋予新值。

与实例字段一样，*objectReference*必须是一个对象的引用。你可以使用一个变量名，但也可以使用任何返回对象引用的表达式。`new`运算符返回一个对象引用，因此你可以使用从`new`返回的值来调用新对象的方法：

```java
new Rectangle(100, 50).getArea()

```

表达式`new Rectangle(100, 50)`返回一个指向`Rectangle`对象的对象引用。如图所示，你可以使用点符号来调用新`Rectangle`的`getArea()`方法来计算新矩形的面积。

一些方法，比如`getArea()`，会返回一个值。对于返回值的方法，你可以在表达式中使用方法调用。你可以将返回值赋给一个变量，用它做决策，或者控制一个循环。这段代码将`getArea()`返回的值赋给变量`areaOfRectangle`：

```java
int areaOfRectangle = new Rectangle(100, 50).getArea();

```

记住，在特定对象上调用方法就相当于向该对象发送消息。在这种情况下，`getArea()`被调用的对象是构造函数返回的矩形。

## 垃圾收集器

一些面向对象的语言要求你跟踪你创建的所有对象，并在不再需要时显式销毁它们。显式管理内存是繁琐且容易出错的。Java 平台允许你创建任意多的对象（当然，受系统处理能力的限制），而且你不必担心销毁它们。Java 运行时环境在确定不再使用对象时删除对象。这个过程称为*垃圾收集*。

当没有更多引用指向对象时，对象就有资格进行垃圾收集。通常，变量中保存的引用在变量超出范围时会被丢弃。或者，你可以通过将变量设置为特殊值`null`来显式丢弃对象引用。记住，一个程序可以有多个引用指向同一个对象；在对象有资格进行垃圾收集之前，所有对对象的引用都必须被丢弃。

Java 运行时环境有一个垃圾收集器，定期释放不再被引用的对象使用的内存。当垃圾收集器确定时机合适时，它会自动执行其任务。
