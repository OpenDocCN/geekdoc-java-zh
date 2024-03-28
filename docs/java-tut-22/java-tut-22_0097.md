# 继承摘要

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/summaryinherit.html`](https://docs.oracle.com/javase/tutorial/java/IandI/summaryinherit.html)

除了`Object`类外，一个类只有一个直接的父类。一个类从所有直接或间接的父类那里继承字段和方法。子类可以重写继承的方法，或者隐藏继承的字段或方法。（请注意，隐藏字段通常是不良的编程实践。）

在覆盖和隐藏方法部分的表格显示了声明具有与超类中方法相同签名的方法的效果。

`Object`类是类层次结构的顶部。所有类都是从这个类继承的后代，并从中继承方法。从`Object`继承的有用方法包括`toString()`，`equals()`，`clone()`和`getClass()`。

通过在类的声明中使用`final`关键字，可以防止类被子类化。同样，通过将方法声明为最终方法，可以防止子类覆盖它。

抽象类只能被子类化；它不能被实例化。抽象类可以包含抽象方法声明但未实现的方法。然后子类为抽象方法提供实现。
