# 创建和使用包的总结

> 原文：[`docs.oracle.com/javase/tutorial/java/package/summary-package.html`](https://docs.oracle.com/javase/tutorial/java/package/summary-package.html)

要为一个类型创建一个包，将一个`package`语句放在包含该类型（类、接口、枚举或注解类型）的源文件中的第一个语句位置。

要使用不同包中的公共类型，你有三种选择：（1）使用类型的完全限定名称，（2）导入类型，或者（3）导入包含该类型的整个包。

包的源文件和类文件的路径名称与包的名称相对应。

你可能需要设置你的`CLASSPATH`，以便编译器和 JVM 可以找到你的类型的`.class`文件。
