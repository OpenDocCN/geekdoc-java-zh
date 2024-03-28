# 匿名类

> 原文：[`docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html`](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html)

匿名类使您的代码更加简洁。它们使您能够同时声明和实例化一个类。它们类似于本地类，只是没有名称。如果您只需要使用本地类一次，请使用它们。

本节涵盖以下主题：

+   声明匿名类

+   匿名类的语法

+   访问封闭范围的本地变量，并声明和访问匿名类的成员

+   匿名类示例

## 声明匿名类

虽然本地类是类声明，匿名类是表达式，这意味着你在另一个表达式中定义类。以下示例，`HelloWorldAnonymousClasses`，在本地变量`frenchGreeting`和`spanishGreeting`的初始化语句中使用了匿名类，但在变量`englishGreeting`的初始化中使用了本地类：

```java

public class HelloWorldAnonymousClasses {

    interface HelloWorld {
        public void greet();
        public void greetSomeone(String someone);
    }

    public void sayHello() {

        class EnglishGreeting implements HelloWorld {
            String name = "world";
            public void greet() {
                greetSomeone("world");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Hello " + name);
            }
        }

        HelloWorld englishGreeting = new EnglishGreeting();

        HelloWorld frenchGreeting = new HelloWorld() {
            String name = "tout le monde";
            public void greet() {
                greetSomeone("tout le monde");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Salut " + name);
            }
        };

        HelloWorld spanishGreeting = new HelloWorld() {
            String name = "mundo";
            public void greet() {
                greetSomeone("mundo");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Hola, " + name);
            }
        };
        englishGreeting.greet();
        frenchGreeting.greetSomeone("Fred");
        spanishGreeting.greet();
    }

    public static void main(String... args) {
        HelloWorldAnonymousClasses myApp =
            new HelloWorldAnonymousClasses();
        myApp.sayHello();
    }            
}

```

## 匿名类的语法

如前所述，匿名类是一个表达式。匿名类表达式的语法类似于构造函数的调用，只是其中包含一个代码块中的类定义。

考虑`frenchGreeting`对象的实例化：

```java
        HelloWorld frenchGreeting = new HelloWorld() {
            String name = "tout le monde";
            public void greet() {
                greetSomeone("tout le monde");
            }
            public void greetSomeone(String someone) {
                name = someone;
                System.out.println("Salut " + name);
            }
        };
```

匿名类表达式包括以下内容：

+   `new`运算符

+   要实现的接口名称或要扩展的类名称。在此示例中，匿名类正在实现接口`HelloWorld`。

+   包含传递给构造函数的参数的括号，就像普通的类实例创建表达式一样。**注意**：当您实现一个接口时，没有构造函数，所以您使用一个空的括号对，就像这个例子中一样。

+   一个类声明体。更具体地说，在类体中，允许方法声明，但不允许语句。

因为匿名类定义是一个表达式，所以它必须是语句的一部分。在此示例中，匿名类表达式是实例化`frenchGreeting`对象的语句的一部分。（这就解释了为什么在右括号后有一个分号。）

## 访问封闭范围的本地变量，并声明和访问匿名类的成员

像本地类一样，匿名类可以捕获变量；它们对封闭范围的本地变量具有相同的访问权限：

+   匿名类可以访问其封闭类的成员。

+   匿名类无法访问其封闭范围中未声明为`final`或有效`final`的本地变量。

+   像嵌套类一样，在匿名类中声明类型（如变量）会遮蔽封闭范围中具有相同名称的任何其他声明。有关更多信息，请参阅遮蔽。

匿名类在成员方面与局部类具有相同的限制：

+   您不能在匿名类中声明静态初始化程序或成员接口。

+   一个匿名类可以有静态成员，只要它们是常量变量。

请注意，您可以在匿名类中声明以下内容：

+   字段

+   额外的方法（即使它们不实现超类型的任何方法）

+   实例初始化程序

+   局部类

然而，你不能在匿名类中声明构造函数。

## 匿名类示例

匿名类经常用于图形用户界面（GUI）应用程序。

考虑 JavaFX 示例[`HelloWorld.java`](https://docs.oracle.com/javase/8/javafx/get-started-tutorial/hello_world.htm)（来自[Hello World, JavaFX Style](https://docs.oracle.com/javase/8/javafx/get-started-tutorial/hello_world.htm)部分，取自[Getting Started with JavaFX](https://docs.oracle.com/javase/8/javafx/get-started-tutorial/javafx_get_started.htm)）。此示例创建一个包含**Say 'Hello World'**按钮的框架。匿名类表达式被突出显示：

```java
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class HelloWorld extends Application {
    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Hello World!");
        Button btn = new Button();
        btn.setText("Say 'Hello World'");
        btn.setOnAction(new EventHandler<ActionEvent>() {

            @Override
            public void handle(ActionEvent event) {
                System.out.println("Hello World!");
            }
        });

        StackPane root = new StackPane();
        root.getChildren().add(btn);
        primaryStage.setScene(new Scene(root, 300, 250));
        primaryStage.show();
    }
}
```

在此示例中，方法调用`btn.setOnAction`指定了当您选择**Say 'Hello World'**按钮时会发生什么。此方法需要一个`EventHandler<ActionEvent>`类型的对象。`EventHandler<ActionEvent>`接口只包含一个方法，即 handle。该示例使用匿名类表达式而不是使用新类来实现此方法。请注意，此表达式是传递给`btn.setOnAction`方法的参数。

因为`EventHandler<ActionEvent>`接口只包含一个方法，所以您可以使用 lambda 表达式代替匿名类表达式。有关更多信息，请参阅 Lambda 表达式部分。

匿名类非常适合实现包含两个或更多方法的接口。以下 JavaFX 示例来自[自定义 UI 控件](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/custom.htm)部分。突出显示的代码创建一个仅接受数字值的文本字段。它通过覆盖从`TextInputControl`类继承的`replaceText`和`replaceSelection`方法，使用匿名类重新定义了`TextField`类的默认实现。

```java
import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.geometry.Insets;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.stage.Stage;

public class CustomTextFieldSample extends Application {

    final static Label label = new Label();

    @Override
    public void start(Stage stage) {
        Group root = new Group();
        Scene scene = new Scene(root, 300, 150);
        stage.setScene(scene);
        stage.setTitle("Text Field Sample");

        GridPane grid = new GridPane();
        grid.setPadding(new Insets(10, 10, 10, 10));
        grid.setVgap(5);
        grid.setHgap(5);

        scene.setRoot(grid);
        final Label dollar = new Label("$");
        GridPane.setConstraints(dollar, 0, 0);
        grid.getChildren().add(dollar);

        final TextField sum = new TextField() {
            @Override
            public void replaceText(int start, int end, String text) {
                if (!text.matches("[a-z, A-Z]")) {
                    super.replaceText(start, end, text); 
                }
                label.setText("Enter a numeric value");
            }

            @Override
            public void replaceSelection(String text) {
                if (!text.matches("[a-z, A-Z]")) {
                    super.replaceSelection(text);
                }
            }
        };

        sum.setPromptText("Enter the total");
        sum.setPrefColumnCount(10);
        GridPane.setConstraints(sum, 1, 0);
        grid.getChildren().add(sum);

        Button submit = new Button("Submit");
        GridPane.setConstraints(submit, 2, 0);
        grid.getChildren().add(submit);

        submit.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent e) {
                label.setText(null);
            }
        });

        GridPane.setConstraints(label, 0, 1);
        GridPane.setColumnSpan(label, 3);
        grid.getChildren().add(label);

        scene.setRoot(grid);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```
