# 命名异常

> 原文：[`docs.oracle.com/javase/tutorial/jndi/ops/exception.html`](https://docs.oracle.com/javase/tutorial/jndi/ops/exception.html)

JNDI 包中的许多方法在需要指示无法执行请求的操作时会抛出[`NamingException`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingException.html)。通常，您会看到围绕可能引发`NamingException`的方法的`try/catch`包装器：

```java
try {
    Context ctx = new InitialContext();
    Object obj = ctx.lookup("somename");
} catch (NamingException e) {
    // Handle the error
    System.err.println(e);
}

```

## 异常类层次结构

JNDI 具有丰富的异常层次结构，源自`NamingException`类。异常的类名是自解释的，并在[此处](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-tree.html)列出。

要特别处理`NamingException`的特定子类，您需要单独捕获子类。例如，以下代码特别处理`AuthenticationException`及其子类。

```java
try {
    Context ctx = new InitialContext();
    Object obj = ctx.lookup("somename");
} catch (AuthenticationException e) {
    // attempt to reacquire the authentication information
    ...
} catch (NamingException e) {
    // Handle the error
    System.err.println(e);
}

```

## 枚举

操作，如[`Context.list()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/Context.html#list-javax.naming.Name-)和[`DirContext.search()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/directory/DirContext.html#search-javax.naming.Name-java.lang.String-javax.naming.directory.SearchControls-)返回一个[`NamingEnumeration`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html)。在这些情况下，如果发生错误并且没有返回结果，则在调用方法时将抛出`NamingException`或其适当的子类。如果发生错误但有一些结果需要返回，则会返回一个`NamingEnumeration`，以便您可以获取这些结果。当所有结果耗尽时，调用[`NamingEnumeration.hasMore()`](https://docs.oracle.com/javase/8/docs/api/javax/naming/NamingEnumeration.html#hasMore--)将导致抛出`NamingException`（或其子类之一）以指示错误。此时，枚举将变为无效状态，不应在其上调用更多方法。

例如，如果执行`search()`并指定要返回多少个答案的计数限制（*n*），则`search()`将返回最多 *n* 个结果的枚举。如果结果数量超过 *n*，则当第 *n+1* 次调用`NamingEnumeration.hasMore()`时，将抛出`SizeLimitExceededException`。请参阅本课程的结果计数示例代码。

## 本教程中的示例

在本教程文本中嵌入的内联示例代码中，通常为了可读性而省略了`try/catch`子句。通常，因为这里只显示代码片段，所以只包含直接用于说明概念的行。如果查看本教程附带的源文件，您将看到用于`NamingException`的`try/catch`子句的适当位置。

javax.naming 包中的异常可以在[这里](https://docs.oracle.com/javase/8/docs/api/javax/naming/package-summary.html)找到。
