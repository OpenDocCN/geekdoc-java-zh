# 细则

> 译文：[`docs.oracle.com/javase/tutorial/extra/generics/fineprint.html`](https://docs.oracle.com/javase/tutorial/extra/generics/fineprint.html)

## 一个泛型类被所有调用共享

以下代码片段打印什么？

```java
List <String> l1 = new ArrayList<String>();
List<Integer> l2 = new ArrayList<Integer>();
System.out.println(l1.getClass() == l2.getClass());

```

你可能会倾向于说`false`，但你会错。它打印`true`，因为泛型类的所有实例具有相同的运行时类，而不考虑它们的实际类型参数。

实际上，使类成为泛型的是它对所有可能的类型参数具有相同行为的事实；同一个类可以被视为具有许多不同的类型。

因此，类的静态变量和方法也被所有实例共享。这就是为什么在静态方法或初始化程序中引用类型声明的类型参数，或在静态变量的声明或初始化程序中引用类型参数是非法的原因。

## 强制转换和 InstanceOf

泛型类被所有实例共享的事实的另一个含义是，通常没有意义询问一个实例是否是泛型类型的特定调用的实例：

```java
Collection cs = new ArrayList<String>();
// *Illegal.*
if (cs instanceof Collection<String>) { ... }

```

同样，像这样的强制转换

```java
// *Unchecked warning,*
Collection<String> cstr = (Collection<String>) cs;

```

给出一个未经检查的警告，因为这不是运行时系统会为你检查的内容。

类型变量也是如此

```java
// *Unchecked warning.* 
<T> T badCast(T t, Object o) {
    return (T) o;
}

```

类型变量在运行时不存在。这意味着它们在时间和空间上都没有性能开销，这很好。不幸的是，这也意味着你不能可靠地在强制转换中使用它们。

## 数组

除非是（无界）通配符类型，否则数组对象的组件类型可能不是类型变量或参数化类型。你可以声明元素类型为类型变量或参数化类型的数组*类型*，但不能声明数组*对象*。

这确实很烦人。这个限制是必要的，以避免出现这样的情况：

```java
// *Not really allowed.*
List<String>[] lsa = new List<String>[10];
Object o = lsa;
Object[] oa = (Object[]) o;
List<Integer> li = new ArrayList<Integer>();
li.add(new Integer(3));
// *Unsound, but passes run time store check*
oa[1] = li;

// *Run-time error: ClassCastException.*
String s = lsa[1].get(0);

```

如果允许参数化类型的数组，前面的例子将在没有任何未经检查警告的情况下编译，并在运行时失败。我们将类型安全作为泛型的主要设计目标。特别是，该语言被设计为保证**如果整个应用程序使用`javac -source 1.5`编译时没有未经检查的警告，那么它是类型安全的**。

然而，你仍然可以使用通配符数组。前面代码的以下变体放弃了使用数组对象和元素类型为参数化的数组类型。因此，我们必须显式转换才能从数组中获取`String`。

```java
// *OK, array of unbounded wildcard type.*
List<?>[] lsa = new List<?>[10];
Object o = lsa;
Object[] oa = (Object[]) o;
List<Integer> li = new ArrayList<Integer>();
li.add(new Integer(3));
// *Correct.*
oa[1] = li;
// *Run time error, but cast is explicit.*
String s = (String) lsa[1].get(0);

```

在下一个变体中，会导致编译时错误，我们避免创建元素类型为参数化的数组对象，但仍然使用具有参数化元素类型的数组类型。

```java
// *Error.*
List<String>[] lsa = new List<?>[10];

```

同样，尝试创建元素类型为类型变量的数组对象会导致编译时错误：

```java
<T> T[] makeArray(T t) {
    return new T[100]; // *Error.*
}

```

由于类型变量在运行时不存在，无法确定实际的数组类型。

解决这类限制的方法是使用类字面量作为运行时类型标记，如下一节所述，类字面量作为运行时类型标记。
