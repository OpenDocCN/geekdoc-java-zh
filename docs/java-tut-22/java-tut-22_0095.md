# 编写最终类和方法

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/final.html`](https://docs.oracle.com/javase/tutorial/java/IandI/final.html)

你可以将类的一些或所有方法声明为*最终*。在方法声明中使用`final`关键字表示该方法不能被子类重写。`Object`类就是这样做的——它的一些方法是`final`的。

如果一个方法有一个不应该被更改的实现，并且对对象的一致状态至关重要，你可能希望将其设置为最终方法。例如，你可能想要将`ChessAlgorithm`类中的`getFirstPlayer`方法设置为最终方法：

```java
class ChessAlgorithm {
    enum ChessPlayer { WHITE, BLACK }
    ...
    final ChessPlayer getFirstPlayer() {
        return ChessPlayer.WHITE;
    }
    ...
}

```

从构造函数中调用的方法通常应该声明为最终方法。如果构造函数调用一个非最终方法，子类可能重新定义该方法，导致意想不到或不希望的结果。

请注意，你也可以声明整个类为最终类。声明为最终类的类不能被子类化。例如，当创建像`String`类这样的不可变类时，这是非常有用的。
