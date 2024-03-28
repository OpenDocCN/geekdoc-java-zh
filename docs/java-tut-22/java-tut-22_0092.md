# 隐藏字段

> 原文：[`docs.oracle.com/javase/tutorial/java/IandI/hidevariables.html`](https://docs.oracle.com/javase/tutorial/java/IandI/hidevariables.html)

在一个类中，如果一个字段与超类中的字段同名，即使它们的类型不同，该字段也会隐藏超类的字段。在子类中，无法通过简单名称引用超类中的字段。相反，必须通过`super`来访问该字段，这将在下一节中介绍。一般来说，我们不建议隐藏字段，因为这会使代码难以阅读。
