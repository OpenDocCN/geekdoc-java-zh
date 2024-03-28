# 接口的演变

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/nogrow.html`](https://docs.oracle.com/javase/tutorial/java/IandI/nogrow.html)

考虑您开发的名为`DoIt`的接口：

```java
public interface DoIt {
   void doSomething(int i, double x);
   int doSomethingElse(String s);
}

```

假设以后，您想要向`DoIt`添加第三个方法，使接口现在变成：

```java
public interface DoIt {

   void doSomething(int i, double x);
   int doSomethingElse(String s);
   boolean didItWork(int i, double x, String s);

}

```

如果您进行此更改，则所有实现旧`DoIt`接口的类都将中断，因为它们不再实现旧接口。依赖于此接口的程序员将会强烈抗议。

尽量预见接口的所有用途并从一开始完全指定它。如果要向接口添加其他方法，您有几个选项。您可以创建一个扩展`DoIt`的`DoItPlus`接口：

```java
public interface DoItPlus extends DoIt {

   boolean didItWork(int i, double x, String s);

}

```

现在，您的代码用户可以选择继续使用旧接口或升级到新接口。

或者，您可以将新方法定义为默认方法。以下示例定义了一个名为`didItWork`的默认方法：

```java
public interface DoIt {

   void doSomething(int i, double x);
   int doSomethingElse(String s);
   default boolean didItWork(int i, double x, String s) {
       // Method body 
   }

}

```

请注意，您必须为默认方法提供实现。您还可以为现有接口定义新的静态方法。实现增强了新默认或静态方法的接口的类的用户无需修改或重新编译它们以适应额外的方法。
