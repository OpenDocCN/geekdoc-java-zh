# 转换现有应用程序

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/converting.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/converting.html)

任何独立的 Java 应用程序或 Java Web Start 应用程序都可以打包为自包含应用程序。如果您有一个 Java 小程序，请参阅[将 Java 小程序重写为 Java Web Start 应用程序](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/applet_dev_guide.html#JSDPG1036)以获取有关将小程序转换为 Java Web Start 应用程序的信息，然后可以将其打包为自包含应用程序。

在转换应用程序之前，请确保您的平台上已安装所需的先决条件。有关信息，请参阅打包自包含应用程序的先决条件。

本节将 Dynamic Tree Demo 从部署 Java Web Start 应用程序转换为自包含应用程序。您可以从自包含应用程序示例下载此演示的源文件。

## 设置目录

确定并组织应用程序所需的文件。一个简单的应用程序可能只需要一个 JAR 文件。一个更复杂的应用程序可能还需要额外的库或资源。自定义资源，如图标或配置文件，也可以被自包含应用程序使用。

Dynamic Tree Demo 只需要`DynamicTreeDemo.jar`文件，该文件位于项目的`/dist`目录中。用于 Java Web Start 版本的应用程序所需的 HTML 和 JNLP 文件不再需要，并且被自包含应用程序的打包工具忽略。

为 Dynamic Tree Demo 提供自定义图标，代表应用程序在用户桌面上安装时的图标，为每个支持的平台提供一个图标。这些图标放置在`/src/package/`platform``目录中。为每个支持的平台提供不同格式的图标：Windows 使用`.ico`格式，Linux 使用`.png`格式，OS X 使用`.icns`格式。

下面的示例显示了在创建自包含捆绑包之前 Dynamic Tree Demo 项目的目录结构：

```java
/packager_DynamicTreeDemo     <--- application project
   /dist
      DynamicTreeDemo.jar
      ...
   /src
      /package                <--- custom resources
         /linux
         /macosx
         /windows
      /webstartComponentArch  <--- application source files
      ...

```

## 设置构建文件

设置所需的打包任务的 Ant 任务。这些任务可以添加到项目的`build.xml`文件中，或放在一个被`build.xml`文件导入的单独文件中。

对于 Dynamic Tree Demo，项目的根目录中的`packager.xml`文件包含了用于生成自包含应用程序包的 Ant 任务。`packager.xml`文件的源代码如下所示：

```java
<project name="DynamicTreePackaging" default="default" basedir="." >
    <echo>${java.home}/../lib/ant-javafx.jar</echo>
    <target name="package" depends="jar">
        <taskdef resource="com/sun/javafx/tools/ant/antlib.xml"
                 uri="javafx:com.sun.javafx.tools.ant"
                 classpath="${java.home}/../lib/ant-javafx.jar;src"/>

        <fx:deploy outdir="${basedir}/build/packager" 
                   outfile="DynamicTreeDemo"
                   nativeBundles="all"
                   verbose="false">

            <fx:application name="Dynamic Tree Demo"
                        mainClass="webstartComponentArch.DynamicTreeApplication"
                        version="1.0"
            />

            <fx:resources>
                <fx:fileset dir="dist" includes="DynamicTreeDemo.jar"/>
            </fx:resources>

            <fx:info title="Dynamic Tree Demo"
                     vendor="My Company"
                     description="A Demo of a Dynamic Swing Tree"
                     category="Demos"
                     copyright="(c) 2014 My Company"
                     license="3 Clause BSD"
            />

            <fx:bundleArgument arg="linux.bundleName" value="dynamic-tree-demo"/>
            <fx:bundleArgument arg="email" value="maintainer@example.com"/>
            <fx:bundleArgument arg="mac.CFBundleName" value="Java Tree Demo"/>
            <fx:bundleArgument arg="win.menuGroup" value="Java Demos"/>

        </fx:deploy>
    </target>
</project>

```

使用以下信息设置 Ant 任务：

+   为命名空间使用`xmlns:fx="javafx:com.sun.javafx.tools.ant`。

+   必须在`fx:deploy`任务之前执行`taskdef`任务。`classpath`属性包含来自 JDK 的`ant-javafx.jar`文件的位置和包含自定义资源的目录。对于动态树演示，`classpath`属性包括包含自定义图标的`/src`目录。

+   将`fx:deploy`任务放在所需的目标内。指定本机二进制文件放置的输出目录，并指定要生成的本机二进制文件。

    如果为本机二进制文件指定了`all`，则将为您在执行此任务文件的平台上生成所有可能的二进制文件，包括磁盘映像。所有平台的有效值为`all`；`image`，在 Windows 和 Linux 上生成文件目录，在 OSX 上生成`.app`文件；以及`installer`，仅为平台生成可安装的捆绑包，而不生成磁盘映像。特定于平台的二进制文件的有效值为 Windows 的`exe`和`msi`；Linux 的`deb`和`rpm`；OS X 的`deb`、`pkg`和`mac.appStore`。您必须安装所需的工具以构建您选择的二进制文件。

    对于动态树演示，`outdir`属性设置为`${basedir}/build/packager`。`basedir`在`project`元素中定义，在本例中设置为当前目录。`nativeBundles`属性设置为`all`，因此将构建在运行打包任务的平台上的所有格式。

+   `verbose`属性是可选的。使用此属性提供诊断信息。

+   提供有关应用程序的信息。在`fx:application`元素的`name`属性和`fx:info`元素的`title`属性中设置应用程序的名称。在`fx:application`元素的`version`属性中设置应用程序的版本。使用`fx:info`元素提供应用程序的描述、供应商名称、许可信息和其他元数据。

+   关于 JAR 文件和其他资源的信息设置在`fx:resources`元素中。

+   启动信息设置在`fx:application`元素的`mainclass`属性中。

    对于动态树演示，使用简单的单个启动器`webstartComponentArch.DynamicTreeApplication`，这是应用程序的主类。

+   其他平台特定的自定义设置在`fx:bundleArgument`元素中提供。未被打包程序识别的参数将被忽略，因此一个构建文件可以包含所有平台的打包信息。

    对于动态树演示，应用了以下自定义设置：

    +   Linux 的捆绑包名称设置为`dynamic-tree-demo`。

    +   提供了一个电子邮件地址。

    +   在 OS X 菜单栏中显示的名称设置为`Java Tree Demo`。

    +   在 Windows 中存储应用程序的菜单组名称设置为`Java Demos`。

## 生成捆绑包

运行您在要为自包含应用程序构建捆绑包的平台上创建的打包任务。

对于动态树演示，请从项目的根目录运行以下命令：

```java
     ant package

```

当打包任务完成时，应用项目中的`build/packager/bundles`目录包含生成的本机二进制文件。

以下示例显示了在为 Windows 生成自包含捆绑包后，动态树演示项目的目录结构：

```java
/packager_DynamicTreeDemo     <--- application project
   /build
      /packager
         /bundles
            Dynamic Tree Demo         <---folder image
            Dynamic Tree Demo-1.0.exe <---EXE installer
            Dynamic Tree Demo-1.0.msi <---MSI installer
      ...   
   /dist
      DynamicTreeDemo.jar
      ...
   /src
      /package                <--- custom resources
         /linux
         /macosx
         /windows
      /webstartComponentArch  <--- application source files
      ...

```

请注意，除了自包含捆绑包之外，打包工具始终会为应用程序生成 JAR、JNLP 和 HTML 文件。这些文件提供了分发应用程序的其他选项。

## 其他参考资料

欲了解更多关于自包含应用程序的信息，请参阅[自包含应用程序打包](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/self-contained-packaging.html)。

欲了解有关 Java 打包工具的 Ant 任务的更多信息，请参阅<macroinline>[JavaFX Ant Tasks](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/javafx_ant_tasks.html)，这些任务用于 Java 和 JavaFX 应用程序。</macroinline>
