# 使用 JNLP API 访问客户端

> 原文：[`docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/usingJNLPAPI.html`](https://docs.oracle.com/javase/tutorial/deployment/doingMoreWithRIA/usingJNLPAPI.html)

当使用 Java 网络启动协议（JNLP）启动时，富互联网应用程序（RIA）可以在用户许可的情况下访问客户端。考虑文本编辑器小程序示例，以了解如何使用基于 JNLP API 的服务。文本编辑器具有文本区域和标有打开、保存和另存为的按钮。文本编辑器可用于打开现有文本文件，编辑它，并将其保存回磁盘。

接下来显示文本编辑器小程序。

//&lt;![CDATA[ var attributes = { code:'TextEditorApplet.class', archive:'examples/dist/applet_JNLP_API/applet_JNLP_API.jar', width:500, height:300} ; var parameters = { jnlp_href: 'examples/dist/applet_JNLP_API/texteditor_applet.jnlp' }; deployJava.runApplet(attributes, parameters, '1.6'); //]]&gt;

* * *

**注意：** 如果您看不到小程序运行，请至少安装[Java SE 开发工具包（JDK）6 更新 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html)版本。

* * *

* * *

**注意：** 如果您看不到示例运行，可能需要在浏览器中启用 JavaScript 解释器，以便部署工具包脚本能够正常运行。

* * *

`TextEditor`和`TextEditorApplet`类布局用户界面并将其显示为小程序。`FileHandler`类包含了关于使用基于 JNLP API 的服务的核心功能。

记住，本主题中描述的技术也适用于 Java Web Start 应用程序。

要使用 JNLP 服务，首先检索对服务的引用。`FileHandler`类的`initialize`方法检索 JNLP 服务的引用，如下面的代码片段所示：

```java
private static synchronized void initialize() {
    ...
    try {
        fos = (FileOpenService)
            ServiceManager.lookup("javax.jnlp.FileOpenService");
        fss = (FileSaveService)
            ServiceManager.lookup("javax.jnlp.FileSaveService");
    } catch (UnavailableServiceException e) {
        ...
    }
}

```

在获得所需服务的引用之后，调用服务上的方法执行必要的操作。`FileHandler`类的`open`方法调用[`FileOpenService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/FileOpenService.html)类的`openFileDialog`方法以显示文件选择器。`open`方法返回所选文件的内容。

```java
public static String open() {
    initialize();
    try {
        fc = fos.openFileDialog(null, null);
        return readFromFile(fc);
    } catch (IOException ioe) {
        ioe.printStackTrace(System.out);
        return null;
    }
}

```

类似地，`FileHandler`类的`save`和`saveAs`方法调用[`FileSaveService`](https://docs.oracle.com/javase/8/docs/jre/api/javaws/jnlp/javax/jnlp/FileSaveService.html)类的相应方法，以使用户能够选择文件名并将文本区域的内容保存到磁盘。

```java
public static void saveAs(String txt) {
    initialize();
    try {
        if (fc == null) {
            // If not already saved.
            // Save-as is like save
            save(txt);
        } else {
            fc = fss.saveAsFileDialog(null, null,
                                         fc);
            save(txt);
        }
    } catch (IOException ioe) {
        ioe.printStackTrace(System.out);
    }
}

```

在运行时，当 RIA 尝试打开或保存文件时，用户会看到一个安全对话框，询问他们是否允许该操作。只有在用户允许 RIA 访问其环境时，操作才会继续进行。

下面显示了`FileHandler`类的完整源代码。

```java

// add javaws.jar to the classpath during compilation 
import javax.jnlp.FileOpenService;
import javax.jnlp.FileSaveService;
import javax.jnlp.FileContents;
import javax.jnlp.ServiceManager;
import javax.jnlp.UnavailableServiceException;
import java.io.*;

public class FileHandler {

    static private FileOpenService fos = null;
    static private FileSaveService fss = null;
    static private FileContents fc = null;

    // retrieves a reference to the JNLP services
    private static synchronized void initialize() {
        if (fss != null) {
            return;
        }
        try {
            fos = (FileOpenService) ServiceManager.lookup("javax.jnlp.FileOpenService");
            fss = (FileSaveService) ServiceManager.lookup("javax.jnlp.FileSaveService");
        } catch (UnavailableServiceException e) {
            fos = null;
            fss = null;
        }
    }

    // displays open file dialog and reads selected file using FileOpenService
    public static String open() {
        initialize();
        try {
            fc = fos.openFileDialog(null, null);
            return readFromFile(fc);
        } catch (IOException ioe) {
            ioe.printStackTrace(System.out);
            return null;
        }
    }

    // displays saveFileDialog and saves file using FileSaveService
    public static void save(String txt) {
        initialize();
        try {
            // Show save dialog if no name is already given
            if (fc == null) {
                fc = fss.saveFileDialog(null, null,
                        new ByteArrayInputStream(txt.getBytes()), null);
                // file saved, done
                return;
            }
            // use this only when filename is known
            if (fc != null) {
                writeToFile(txt, fc);
            }
        } catch (IOException ioe) {
            ioe.printStackTrace(System.out);
        }
    }

    // displays saveAsFileDialog and saves file using FileSaveService
    public static void saveAs(String txt) {
        initialize();
        try {
            if (fc == null) {
                // If not already saved. Save-as is like save
                save(txt);
            } else {
                fc = fss.saveAsFileDialog(null, null, fc);
                save(txt);
            }
        } catch (IOException ioe) {
            ioe.printStackTrace(System.out);
        }
    }

    private static void writeToFile(String txt, FileContents fc) throws IOException {
        int sizeNeeded = txt.length() * 2;
        if (sizeNeeded > fc.getMaxLength()) {
            fc.setMaxLength(sizeNeeded);
        }
        BufferedWriter os = new BufferedWriter(new OutputStreamWriter(fc.getOutputStream(true)));
        os.write(txt);
        os.close();
    }

    private static String readFromFile(FileContents fc) throws IOException {
        if (fc == null) {
            return null;
        }
        BufferedReader br = new BufferedReader(new InputStreamReader(fc.getInputStream()));
        StringBuffer sb = new StringBuffer((int) fc.getLength());
        String line = br.readLine();
        while (line != null) {
            sb.append(line);
            sb.append("\n");
            line = br.readLine();
        }
        br.close();
        return sb.toString();
    }
}

```

* * *

**注意：** 要编译具有对`javax.jnlp`包中类的引用的 Java 代码，请在类路径中包含`<你的 JDK 路径>/jre/lib/javaws.jar`。在运行时，Java 运行环境软件会自动使这些类对 RIA 可用。

* * *

下载源代码以进一步尝试*文本编辑器小程序*示例。
