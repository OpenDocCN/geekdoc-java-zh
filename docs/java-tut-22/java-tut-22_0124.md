# 通配符

> 原文：[`docs.oracle.com/javase/tutorial/java/generics/wildcards.html`](https://docs.oracle.com/javase/tutorial/java/generics/wildcards.html)

在泛型代码中，问号（`?`），称为*通配符*，表示未知类型。 通配符可以在各种情况下使用：作为参数、字段或局部变量的类型；有时作为返回类型（尽管更具体的编程实践更好）。 通配符永远不会用作泛型方法调用、泛型类实例创建或超类型的类型参数。

以下部分将更详细地讨论通配符，包括上界通配符、下界通配符和通配符捕获。
