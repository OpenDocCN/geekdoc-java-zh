# 创建一个包

> 原文：[`docs.oracle.com/javase/tutorial/java/package/createpkgs.html`](https://docs.oracle.com/javase/tutorial/java/package/createpkgs.html)

要创建一个包，你需要为包选择一个名称（命名约定将在下一节讨论），并在包含你想要放入包中的类型（类、接口、枚举和注解类型）的*每个源文件*的顶部放置一个带有该名称的`package`语句。

包语句（例如，`package graphics;`）必须是源文件中的第一行。每个源文件中只能有一个包语句，并且它适用于文件中的所有类型。

* * *

**注意：**如果你在单个源文件中放入多个类型，只能有一个是`public`的，并且它必须与源文件同名。例如，你可以在文件`Circle.java`中定义`public class Circle`，在文件`Draggable.java`中定义`public interface Draggable`，在文件`Day.java`中定义`public enum Day`，等等。

你可以在同一个文件中包含非公共类型和一个公共类型（这是强烈不推荐的，除非非公共类型很小并且与公共类型密切相关），但只有公共类型可以从包外访问。所有顶级的非公共类型将是*包私有*的。

* * *

如果你将前面部分列出的图形接口和类放在一个名为`graphics`的包中，你将需要六个源文件，如下所示：

```java
//*in the Draggable.java file*
package graphics;
public interface Draggable {
    . . .
}

//*in the Graphic.java file*
package graphics;
public abstract class Graphic {
    . . .
}

//*in the Circle.java file*
package graphics;
public class Circle extends Graphic
    implements Draggable {
    . . .
}

//*in the Rectangle.java file*
package graphics;
public class Rectangle extends Graphic
    implements Draggable {
    . . .
}

//*in the Point.java file*
package graphics;
public class Point extends Graphic
    implements Draggable {
    . . .
}

//*in the Line.java file*
package graphics;
public class Line extends Graphic
    implements Draggable {
    . . .
}

```

如果你不使用`package`语句，你的类型将会进入一个无名包。一般来说，无名包只适用于小型或临时应用程序，或者当你刚开始开发过程时。否则，类和接口应该放在命名包中。
