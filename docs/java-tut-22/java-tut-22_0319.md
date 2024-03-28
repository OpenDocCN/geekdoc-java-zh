# 打包自包含应用程序的先决条件

> 原文：[`docs.oracle.com/javase/tutorial/deployment/selfContainedApps/prereqs.html`](https://docs.oracle.com/javase/tutorial/deployment/selfContainedApps/prereqs.html)

编译和打包应用程序需要 Java 开发工具包（JDK）。可安装的捆绑包必须在自包含应用程序将运行的平台上创建。例如，如果您的应用程序在 Windows 和 Linux 上运行，您必须在 Windows 上运行打包工具来创建`.exe`或`.msi`捆绑包，并在 Linux 上运行打包工具来创建`.rpm`或`.deb`文件。

创建可安装的捆绑包需要第三方工具。以下表格标识了每个支持平台的工具。

| 平台 | 格式 | 工具 |
| --- | --- | --- |
| Windows | EXE | Inno Setup 5 或更高版本 |
| Windows | MSI | WiX Toolset 3.8 或更高版本 |
| Linux | RPM | RPMBuild |
| Linux | DEB | Debian 打包工具 |
| OS X | DMG |   |
| OS X | PKG |   |
