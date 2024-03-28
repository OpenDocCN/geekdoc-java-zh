# 从 JavaScript 代码调用 Applet 方法

> 原文：[`docs.oracle.com/javase/tutorial/deployment/applet/invokingAppletMethodsFromJavaScript.html`](https://docs.oracle.com/javase/tutorial/deployment/applet/invokingAppletMethodsFromJavaScript.html)

网页上的 JavaScript 代码可以与嵌入在页面上的 Java applet 进行交互。JavaScript 代码可以执行以下操作：

+   调用 Java 对象上的方法

+   获取并设置 Java 对象中的字段

+   获取和设置 Java 数组元素

[LiveConnect 规范](http://www.oracle.com/technetwork/java/javase/plugin2-142482.html#LIVECONNECT)描述了 JavaScript 代码如何与 Java 代码通信的详细信息。

当 JavaScript 代码调用 Java applet 时，会显示安全警告。要抑制这些警告，需要在 JAR 文件清单中添加`Caller-Allowable-Codebase`属性。指定允许调用 applet 的 JavaScript 代码的位置。有关`Caller-Allowable-Codebase`属性的信息，请参阅[安全的 JAR 文件清单属性](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/manifest.html)。

本主题探讨了使用 Math applet 示例进行 JavaScript 代码到 Java applet 通信。`MathApplet`类和支持的`Calculator`类提供了一组公共方法和变量。网页上的 JavaScript 代码调用和评估这些公共成员以传递数据并检索计算结果。

## Math Applet 和相关类

这是``MathApplet``类的源代码。`getCalculator`方法返回`Calculator`辅助类的引用。

```java

package jstojava;
import java.applet.Applet;

public class MathApplet extends Applet{

    public String userName = null;

    public String getGreeting() {
        return "Hello " + userName;
    }

    public Calculator getCalculator() {
        return new Calculator();
    } 

    public DateHelper getDateHelper() {
        return new DateHelper();
    }

    public void printOut(String text) {
        System.out.println(text);
    }
}

```

``Calculator``类中的方法允许用户设置两个值，相加数字，并在范围内检索数字。

```java

package jstojava;

public class Calculator {
    private int a = 0;
    private int b = 0; // assume b > a

    public void setNums(int numA, int numB) {
        a = numA;
        b = numB;
    }

    public int add() {
        return a + b;
    }

    public int [] getNumInRange() {    
        int x = a;
        int len = (b - a) + 1;
        int [] range = new int [len];
        for (int i = 0; i < len; i++) {
            range[i]= x++;
            System.out.println("i: " + i + " ; range[i]: " + range[i]);
        }
        return range;
    }
}

```

``DateHelper``类的`getDate`方法返回当前日期。

```java

package jstojava;
import java.util.Date;
import java.text.SimpleDateFormat;

public class DateHelper {

    public static String label = null;

    public String getDate() {
        return label + " " + new SimpleDateFormat().format(new Date());
    }

}

```

## 部署 Applet

在网页中部署 applet，``AppletPage.html``。部署 applet 时，请确保为 applet 指定一个 id。稍后将使用 applet id 获取对 applet 对象的引用。

```java
<script src=
  "https://www.java.com/js/deployJava.js"></script>
<script>
    <!-- applet id can be used to get a reference to
    the applet object -->
    var attributes = { id:'mathApplet',
        code:'jstojava.MathApplet',  width:1, height:1} ;
    var parameters = { jnlp_href: 'math_applet.jnlp'} ;
    deployJava.runApplet(attributes, parameters, '1.6');
</script>

```

接下来，在``AppletPage.html``网页中添加一些 JavaScript 代码。JavaScript 代码可以使用 applet id 作为对 applet 对象的引用，并调用 applet 的方法。在下面的示例中，JavaScript 代码设置了 applet 的公共成员变量，调用了公共方法，并检索了由 applet 引用的另一个对象（`Calculator`）的引用。JavaScript 代码能够处理基本类型、数组和对象返回类型。

```java
<script language="javascript">
    function enterNums(){
        var numA = prompt('Enter number \'a\'?','0');
        var numB = prompt(
            'Enter number \'b\' (should be greater than number \'a\' ?','1');
        // set applet's public variable
        mathApplet.userName = "John Doe";

        // invoke public applet method
        var greeting = mathApplet.getGreeting();

        // get another class referenced by applet and
        // invoke its methods
        var calculator = mathApplet.getCalculator();
        calculator.setNums(numA, numB);

        // primitive datatype returned by applet
        var sum = calculator.add();

        // array returned by applet
        var numRange = calculator.getNumInRange();

        // check Java console log for this message
        mathApplet.printOut("Testing printing to System.out");

        // get another class, set static field and invoke its methods
        var dateHelper = mathApplet.getDateHelper();
        dateHelper.label = "Today\'s date is: ";
        var dateStr = dateHelper.getDate();
        <!-- ... -->
</script>

```

当数字 a = 0 和 b = 5 时，Math applet 在网页上显示以下结果：

```java
Results of JavaScript to Java Communication

Hello John Doe

a = 0 ; b = 5

Sum: 5

Numbers in range array: [ 0, 1, 2, 3, 4, 5 ]

Today's date is: 5/28/13 4:12 PM //*shows current date*

```

在浏览器中打开``AppletPage.html``以查看数学小程序。

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE 开发工具包（JDK）6 更新 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果您看不到示例运行，请确保在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

检查由 JavaScript 代码调用的小程序所受到的安全限制。

下载源代码以进一步进行实验的*通过 JavaScript 代码调用小程序方法*示例。
