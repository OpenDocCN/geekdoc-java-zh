# 使用接口作为类型

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/interfaceAsType.html`](https://docs.oracle.com/javase/tutorial/java/IandI/interfaceAsType.html)

当你定义一个新接口时，你正在定义一个新的引用数据类型。你可以在任何可以使用其他数据类型名称的地方使用接口名称。如果你定义一个类型为接口的引用变量，那么你分配给它的任何对象*必须*是实现了该接口的类的实例。

举例来说，这里有一种方法可以找到一对对象中最大的对象，适用于*任何*从实现了`Relatable`接口的类实例化的对象：

```java
public Object findLargest(Object object1, Object object2) {
   Relatable obj1 = (Relatable)object1;
   Relatable obj2 = (Relatable)object2;
   if ((obj1).isLargerThan(obj2) > 0)
      return object1;
   else 
      return object2;
}

```

通过将`object1`强制转换为`Relatable`类型，它可以调用`isLargerThan`方法。

如果你坚持在各种类中实现`Relatable`，那么从*任何*这些类实例化的对象都可以使用`findLargest()`方法进行比较——前提是这两个对象属于同一类。同样，它们也可以使用以下方法进行比较：

```java
public Object findSmallest(Object object1, Object object2) {
   Relatable obj1 = (Relatable)object1;
   Relatable obj2 = (Relatable)object2;
   if ((obj1).isLargerThan(obj2) < 0)
      return object1;
   else 
      return object2;
}

public boolean isEqual(Object object1, Object object2) {
   Relatable obj1 = (Relatable)object1;
   Relatable obj2 = (Relatable)object2;
   if ( (obj1).isLargerThan(obj2) == 0)
      return true;
   else 
      return false;
}

```

这些方法适用于任何“可比较”的对象，无论它们的类继承关系如何。当它们实现了`Relatable`接口时，它们可以是自己类（或超类）类型和`Relatable`类型。这使它们具有多重继承的一些优势，可以同时具有来自超类和接口的行为。
