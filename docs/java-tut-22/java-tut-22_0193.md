# 传统文件 I/O 代码

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/legacy.html`](https://docs.oracle.com/javase/tutorial/essential/io/legacy.html)

## 与旧代码的互操作性

在 Java SE 7 发布之前，`java.io.File` 类是文件 I/O 的机制，但它有一些缺点。

+   许多方法在失败时不会抛出异常，因此无法获得有用的错误消息。例如，如果文件删除失败，程序将收到“删除失败”，但不知道是因为文件不存在、用户没有权限还是其他问题。

+   `rename` 方法在各个平台上的工作不一致。

+   没有对符号链接的真正支持。

+   需要更多对元数据的支持，如文件权限、文件所有者和其他安全属性。

+   访问文件元数据效率低下。

+   许多 `File` 方法不具备可扩展性。在服务器上请求大型目录列表可能导致挂起。大型目录也可能导致内存资源问题，导致拒绝服务。

+   不可能编写可靠的代码，可以递归遍历文件树，并在存在循环符号链接时做出适当响应。

也许您有使用 `java.io.File` 的旧代码，并希望最小影响地利用 `java.nio.file.Path` 功能。

`java.io.File` 类提供了 [`toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--) 方法，将旧式 `File` 实例转换为 `java.nio.file.Path` 实例，如下所示：

```java
Path input = file.toPath();

```

然后，您可以利用 `Path` 类提供的丰富功能集。

例如，假设您有一些删除文件的代码：

```java
file.delete();

```

您可以修改此代码以使用 `Files.delete` 方法，如下所示：

```java
Path fp = file.toPath();
Files.delete(fp);

```

相反，[`Path.toFile`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#toFile--) 方法为 `Path` 对象构造一个 `java.io.File` 对象。

## 将 java.io.File 功能映射到 java.nio.file

由于 Java SE 7 发布中的文件 I/O 实现已完全重新架构，因此不能将一个方法替换为另一个方法。如果您想使用 `java.nio.file` 包提供的丰富功能，最简单的解决方案是使用前一节中建议的 [`File.toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--) 方法。但是，如果您不想使用该方法或该方法不符合您的需求，您必须重写文件 I/O 代码。

两个 API 之间没有一对一对应关系，但以下表格给出了 `java.io.File` API 中的功能在 `java.nio.file` API 中的映射，并告诉您可以在哪里获取更多信息。

| java.io.File 功能 | java.nio.file 功能 | 教程覆盖范围 |
| --- | --- | --- |
| `java.io.File` | `java.nio.file.Path` | Path 类 |
| `java.io.RandomAccessFile` | `SeekableByteChannel` 功能。 | 随机访问文件 |
| `File.canRead`、`canWrite`、`canExecute` | `Files.isReadable`、`Files.isWritable` 和 `Files.isExecutable`。在 UNIX 文件系统上，使用 管理元数据（文件和文件存储属性） 包来检查九个文件权限。 | 检查文件或目录 管理元数据 |
| `File.isDirectory()`、`File.isFile()` 和 `File.length()` | `Files.isDirectory(Path, LinkOption...)`、`Files.isRegularFile(Path, LinkOption...)` 和 `Files.size(Path)` | 管理元数据 |
| `File.lastModified()` 和 `File.setLastModified(long)` | `Files.getLastModifiedTime(Path, LinkOption...)` 和 `Files.setLastMOdifiedTime(Path, FileTime)` | 管理元数据 |
| 设置各种属性的 `File` 方法：`setExecutable`、`setReadable`、`setReadOnly`、`setWritable` | 这些方法被 `Files` 方法 `setAttribute(Path, String, Object, LinkOption...)` 替代。 | 管理元数据 |
| `new File(parent, "newfile")` | `parent.resolve("newfile")` | 路径操作 |
| `File.renameTo` | `Files.move` | 移动文件或目录 |
| `File.delete` | `Files.delete` | 删除文件或目录 |
| `File.createNewFile` | `Files.createFile` | 创建文件 |
| `File.deleteOnExit` | 由 `createFile` 方法中指定的 `DELETE_ON_CLOSE` 选项替代。 | 创建文件 |

| `File.createTempFile` | `Files.createTempFile(Path, String, FileAttributes<?>)`、`Files.createTempFile(Path, String, String, FileAttributes<?>)` | 创建文件 通过流 I/O 创建和写入文件

通过通道 I/O 读写文件 |

| `File.exists` | `Files.exists` 和 `Files.notExists` | 验证文件或目录的存在性 |
| --- | --- | --- |
| `File.compareTo` and `equals` | `Path.compareTo` and `equals` | 比较两个路径 |
| `File.getAbsolutePath` and `getAbsoluteFile` | `Path.toAbsolutePath` | 转换路径 |

| `File.getCanonicalPath` and `getCanonicalFile` | `Path.toRealPath` 或 `normalize` | 转换路径 (`toRealPath`) 从路径中删除冗余部分 (`normalize`)

|

| `File.toURI` | `Path.toURI` | 转换路径 |
| --- | --- | --- |
| `File.isHidden` | `Files.isHidden` | 检索路径信息 |
| `File.list` and `listFiles` | `Path.newDirectoryStream` | 列出目录内容 |
| `File.mkdir` 和 `mkdirs` | `Files.createDirectory` | 创建目录 |
| `File.listRoots` | `FileSystem.getRootDirectories` | 列出文件系统的根目录 |
| `File.getTotalSpace`、`File.getFreeSpace`、`File.getUsableSpace` | `FileStore.getTotalSpace`、`FileStore.getUnallocatedSpace`、`FileStore.getUsableSpace`、`FileStore.getTotalSpace` | 文件存储属性 |
