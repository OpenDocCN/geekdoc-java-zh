# 命令行 I/O

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/cl.html`](https://docs.oracle.com/javase/tutorial/essential/io/cl.html)

程序通常从命令行运行并在命令行环境中与用户交互。Java 平台通过标准流和控制台两种方式支持这种交互。

## 标准流

标准流是许多操作系统的特性。默认情况下，它们从键盘读取输入并将输出写入显示器。它们还支持文件和程序之间的 I/O，但该功能由命令行解释器控制，而不是程序。

Java 平台支持三个标准流：*标准输入*，通过 `System.in` 访问；*标准输出*，通过 `System.out` 访问；以及*标准错误*，通过 `System.err` 访问。这些对象会自动定义，无需打开。标准输出和标准错误都用于输出；将错误输出单独处理允许用户将常规输出重定向到文件并仍能读取错误消息。有关更多信息，请参考您的命令行解释器文档。

你可能期望标准流是字符流，但出于历史原因，它们是字节流。`System.out` 和 `System.err` 被定义为 [`PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html) 对象。虽然技术上是字节流，但 `PrintStream` 利用内部字符流对象来模拟许多字符流的特性。

相比之下，`System.in` 是一个没有字符流特性的字节流。要将标准输入作为字符流使用，需要将 `System.in` 包装在 `InputStreamReader` 中。

```java
InputStreamReader cin = new InputStreamReader(System.in);

```

## 控制台

比标准流更高级的替代方案是控制台。这是一种类型为 [`Console`](https://docs.oracle.com/javase/8/docs/api/java/io/Console.html) 的单一预定义对象，具有标准流提供的大部分功能，以及其他功能。控制台特别适用于安全密码输入。控制台对象还通过其 `reader` 和 `writer` 方法提供真正的字符流输入和输出流。

在程序可以使用控制台之前，必须通过调用 `System.console()` 尝试检索控制台对象。如果控制台对象可用，则此方法将返回它。如果 `System.console` 返回 `NULL`，则不允许控制台操作，可能是因为操作系统不支持它们或者因为程序在非交互环境中启动。

控制台对象通过其`readPassword`方法支持安全密码输入。该方法通过两种方式帮助安全密码输入。首先，它抑制回显，因此密码不会在用户屏幕上可见。其次，`readPassword`返回一个字符数组，而不是一个`String`，因此密码可以被覆盖，一旦不再需要，即从内存中删除。

`Password`示例是一个用于更改用户密码的原型程序。它演示了几种`Console`方法。

```java
import java.io.Console;
import java.util.Arrays;
import java.io.IOException;

public class Password {

    public static void main (String args[]) throws IOException {

        Console c = System.console();
        if (c == null) {
            System.err.println("No console.");
            System.exit(1);
        }

        String login = c.readLine("Enter your login: ");
        char [] oldPassword = c.readPassword("Enter your old password: ");

        if (verify(login, oldPassword)) {
            boolean noMatch;
            do {
                char [] newPassword1 = c.readPassword("Enter your new password: ");
                char [] newPassword2 = c.readPassword("Enter new password again: ");
                noMatch = ! Arrays.equals(newPassword1, newPassword2);
                if (noMatch) {
                    c.format("Passwords don't match. Try again.%n");
                } else {
                    change(login, newPassword1);
                    c.format("Password for %s changed.%n", login);
                }
                Arrays.fill(newPassword1, ' ');
                Arrays.fill(newPassword2, ' ');
            } while (noMatch);
        }

        Arrays.fill(oldPassword, ' ');
    }

    // Dummy change method.
    static boolean verify(String login, char[] password) {
        // This method always returns
        // true in this example.
        // Modify this method to verify
        // password according to your rules.
        return true;
    }

    // Dummy change method.
    static void change(String login, char[] password) {
        // Modify this method to change
        // password according to your rules.
    }
}

```

`Password`类遵循以下步骤：

1.  尝试检索控制台对象。如果对象不可用，则中止。

1.  调用`Console.readLine`提示并读取用户的登录名。

1.  调用`Console.readPassword`提示并读取用户的现有密码。

1.  调用`verify`确认用户有权限更改密码。（在这个例子中，`verify`是一个始终返回`true`的虚拟方法。）

1.  重复以下步骤，直到用户两次输入相同的密码：

    1.  两次调用`Console.readPassword`提示并读取新密码。

    1.  如果用户两次输入相同的密码，调用`change`进行更改。（同样，`change`是一个虚拟方法。）

    1.  用空格覆盖两个密码。

1.  用空格覆盖旧密码。
