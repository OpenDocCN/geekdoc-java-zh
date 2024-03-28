# 通用方法

> 原文：[`docs.oracle.com/javase/tutorial/extra/generics/methods.html`](https://docs.oracle.com/javase/tutorial/extra/generics/methods.html)

考虑编写一个方法，该方法接受一个对象数组和一个集合，并将数组中的所有对象放入集合中。以下是第一次尝试：

```java
static void fromArrayToCollection(Object[] a, Collection<?> c) {
    for (Object o : a) { 
        c.add(o); // *compile-time error*
    }
}

```

到目前为止，您已经学会了避免初学者的错误，即尝试将`Collection<Object>`用作集合参数的类型。您可能已经意识到，使用`Collection<?>`也行不通。请记住，您不能将对象随意放入未知类型的集合中。

处理这些问题的方法是使用*通用方法*。就像类型声明一样，方法声明也可以是通用的即，由一个或多个类型参数参数化。

```java
static <T> void fromArrayToCollection(T[] a, Collection<T> c) {
    for (T o : a) {
        c.add(o); // *Correct*
    }
}

```

我们可以使用任何元素类型为数组元素类型的超类型的任何类型的集合调用此方法。

```java
Object[] oa = new Object[100];
Collection<Object> co = new ArrayList<Object>();

// *T inferred to be Object*
fromArrayToCollection(oa, co); 

String[] sa = new String[100];
Collection<String> cs = new ArrayList<String>();

// *T inferred to be String*
fromArrayToCollection(sa, cs);

// *T inferred to be Object*
fromArrayToCollection(sa, co);

Integer[] ia = new Integer[100];
Float[] fa = new Float[100];
Number[] na = new Number[100];
Collection<Number> cn = new ArrayList<Number>();

// *T inferred to be Number*
fromArrayToCollection(ia, cn);

// *T inferred to be Number*
fromArrayToCollection(fa, cn);

// *T inferred to be Number*
fromArrayToCollection(na, cn);

// *T inferred to be Object*
fromArrayToCollection(na, co);

// *compile-time error*
fromArrayToCollection(na, cs);

```

请注意，我们不必向通用方法传递实际类型参数。编译器根据实际参数的类型为我们推断类型参数。它通常会推断使调用类型正确的最具体的类型参数。

一个问题是：何时应该使用通用方法，何时应该使用通配符类型？为了理解答案，让我们来看看`Collection`库中的一些方法。

```java
interface Collection<E> {
    public boolean containsAll(Collection<?> c);
    public boolean addAll(Collection<? extends E> c);
}

```

我们可以在这里使用通用方法：

```java
interface Collection<E> {
    public <T> boolean containsAll(Collection<T> c);
    public <T extends E> boolean addAll(Collection<T> c);
    // *Hey, type variables can have bounds too!*
}

```

然而，在`containsAll`和`addAll`中，类型参数`T`仅使用一次。返回类型不依赖于类型参数，方法的任何其他参数也不依赖于类型参数（在这种情况下，只有一个参数）。这告诉我们，类型参数用于多态性；它的唯一效果是允许在不同的调用站点使用各种实际参数类型。如果是这种情况，应该使用通配符。通配符旨在支持灵活的子类型化，这正是我们要表达的。

通用方法允许使用类型参数来表示方法的一个或多个参数以及/或其返回类型之间的依赖关系。如果没有这样的依赖关系，则不应使用通用方法。

可以同时使用通用方法和通配符。以下是`Collections.copy()`方法的使用方法：

```java
class Collections {
    public static <T> void copy(List<T> dest, List<? extends T> src) {
    ...
}

```

注意两个参数类型之间的依赖关系。从源列表`src`复制的任何对象必须可分配给目标列表`dst`的元素类型`T`。因此，`src`的元素类型可以是`T`的任何子类型我们不关心是哪种类型。`copy`的签名使用类型参数表达依赖关系，但对第二个参数的元素类型使用通配符。

我们可以以另一种方式编写此方法的签名，而根本不使用通配符：

```java
class Collections {
    public static <T, S extends T> void copy(List<T> dest, List<S> src) {
    ...
}

```

这是可以的，但是第一个类型参数既用于`dst`的类型，也用于第二个类型参数`S`的边界，而`S`本身只在`src`的类型中使用了一次没有其他地方依赖于它。这表明我们可以用通配符替换`S`。使用通配符比声明显式类型参数更清晰、更简洁，因此在可能的情况下应优先使用通配符。

通配符还有一个优点，就是它们可以在方法签名之外使用，比如字段、局部变量和数组的类型。这里是一个例子。

回到我们的形状绘制问题，假设我们想要保留绘制请求的历史记录。我们可以在`Shape`类内部的静态变量中维护历史记录，并让`drawAll()`将其传入的参数存储到历史字段中。

```java
static List<List<? extends Shape>> 
    history = new ArrayList<List<? extends Shape>>();

public void drawAll(List<? extends Shape> shapes) {
    history.addLast(shapes);
    for (Shape s: shapes) {
        s.draw(this);
    }
}

```

最后，让我们再次注意一下用于类型参数的命名约定。我们在没有更具体的类型来区分时使用`T`表示类型。这在泛型方法中经常发生。如果有多个类型参数，我们可能会使用字母来区分`T`在字母表中的邻居，比如`S`。如果泛型方法出现在泛型类中，最好避免在方法和类的类型参数中使用相同的名称，以避免混淆。嵌套泛型类也适用相同的规则。
