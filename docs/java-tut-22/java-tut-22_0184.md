# 管理元数据（文件和文件存储属性）

> 原文：[`docs.oracle.com/javase/tutorial/essential/io/fileAttr.html`](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)

*元数据*的定义是“关于其他数据的数据”。在文件系统中，数据包含在其文件和目录中，元数据跟踪每个对象的信息：它是一个常规文件、目录还是链接？它的大小、创建日期、最后修改日期、文件所有者、组所有者和访问权限是什么？

文件系统的元数据通常被称为其*文件属性*。`Files`类包括可用于获取文件的单个属性或设置属性的方法。

| 方法 | 说明 |
| --- | --- |
| [`size(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#size-java.nio.file.Path-) | 返回指定文件的大小（以字节为单位）。 |
| [`isDirectory(Path, LinkOption)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isDirectory-java.nio.file.Path-java.nio.file.LinkOption...-) | 如果指定的`Path`定位到一个目录，则返回 true。 |
| [`isRegularFile(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isRegularFile-java.nio.file.Path-java.nio.file.LinkOption...-) | 如果指定的`Path`定位到一个常规文件，则返回 true。 |
| [`isSymbolicLink(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isSymbolicLink-java.nio.file.Path-) | 如果指定的`Path`定位到一个符号链接文件，则返回 true。 |
| [`isHidden(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isHidden-java.nio.file.Path-) | 如果指定的`Path`定位到文件系统认为是隐藏的文件，则返回 true。 |
| [`getLastModifiedTime(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getLastModifiedTime-java.nio.file.Path-java.nio.file.LinkOption...-) [`setLastModifiedTime(Path, FileTime)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setLastModifiedTime-java.nio.file.Path-java.nio.file.attribute.FileTime-) | 返回或设置指定文件的最后修改时间。 |
| [`getOwner(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getOwner-java.nio.file.Path-java.nio.file.LinkOption...-) [`setOwner(Path, UserPrincipal)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setOwner-java.nio.file.Path-java.nio.file.attribute.UserPrincipal-) | 返回或设置文件的所有者。 |
| [`getPosixFilePermissions(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getPosixFilePermissions-java.nio.file.Path-java.nio.file.LinkOption...-) [`setPosixFilePermissions(Path, Set<PosixFilePermission>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setPosixFilePermissions-java.nio.file.Path-java.util.Set-) | 返回或设置文件的 POSIX 文件权限。 |
| [`getAttribute(Path, String, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getAttribute-java.nio.file.Path-java.lang.String-java.nio.file.LinkOption...-) [`setAttribute(Path, String, Object, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setAttribute-java.nio.file.Path-java.lang.String-java.lang.Object-java.nio.file.LinkOption...-) | 返回或设置文件属性的值。 |

如果程序需要同时获取多个文件属性，使用检索单个属性的方法可能效率低下。反复访问文件系统以检索单个属性可能会对性能产生不利影响。因此，`Files` 类提供了两个 `readAttributes` 方法，以便一次性获取文件的属性。

| 方法 | 注释 |
| --- | --- |
| [`readAttributes(Path, String, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readAttributes-java.nio.file.Path-java.lang.String-java.nio.file.LinkOption...-) | 作为批量操作读取文件的属性。`String` 参数标识要读取的属性。 |
| [`readAttributes(Path, Class<A>, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readAttributes-java.nio.file.Path-java.lang.Class-java.nio.file.LinkOption...-) | 作为批量操作读取文件的属性。`Class<A>` 参数是请求的属性类型，该方法返回该类的对象。 |

在展示 `readAttributes` 方法的示例之前，应该提到不同的文件系统对应该跟踪哪些属性有不同的概念。因此，相关文件属性被分组到视图中。*视图* 映射到特定的文件系统实现，如 POSIX 或 DOS，或者映射到常见功能，如文件所有权。

支持的视图如下：

+   [`BasicFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/BasicFileAttributeView.html) – 提供必须由所有文件系统实现支持的基本属性视图。

+   [`DosFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/DosFileAttributeView.html) – 将基本属性视图扩展为在支持 DOS 属性的文件系统上支持的标准四位。

+   [`PosixFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/PosixFileAttributeView.html) – 将基本属性视图扩展到支持 POSIX 标准族的文件系统上支持的属性，例如 UNIX。这些属性包括文件所有者、组所有者和九个相关的访问权限。

+   [`FileOwnerAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/FileOwnerAttributeView.html) – 受到支持文件所有者概念的任何文件系统实现支持。

+   [`AclFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/AclFileAttributeView.html) – 支持读取或更新文件的访问控制列表（ACL）。支持 NFSv4 ACL 模型。任何 ACL 模型，例如 Windows ACL 模型，如果与 NFSv4 模型有明确定义的映射，也可能受支持。

+   [`UserDefinedFileAttributeView`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/UserDefinedFileAttributeView.html) – 支持用户定义的元数据。此视图可以映射到系统支持的任何扩展机制。例如，在 Solaris 操作系统中，您可以使用此视图存储文件的 MIME 类型。

特定的文件系统实现可能仅支持基本文件属性视图，或者可能支持其中几个文件属性视图。文件系统实现可能支持此 API 中未包含的其他属性视图。

在大多数情况下，您不应直接处理任何 `FileAttributeView` 接口。（如果您确实需要直接使用 `FileAttributeView`，可以通过 [`getFileAttributeView(Path, Class<V>, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getFileAttributeView-java.nio.file.Path-java.lang.Class-java.nio.file.LinkOption...-) 方法访问它。）

`readAttributes` 方法使用泛型，可用于读取任何文件属性视图的属性。本页其余部分的示例使用 `readAttributes` 方法。

本节的其余部分涵盖以下主题：

+   基本文件属性

+   设置时间戳

+   DOS 文件属性

+   POSIX 文件权限

+   设置文件或组所有者

+   用户定义的文件属性

+   文件存储属性

## 基本文件属性

如前所述，要读取文件的基本属性，可以使用 `Files.readAttributes` 方法之一，该方法一次性读取所有基本属性。这比单独访问文件系统以读取每个单独属性要高效得多。可变参数参数当前支持 [`LinkOption`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/LinkOption.html) 枚举，`NOFOLLOW_LINKS`。当您不希望跟随符号链接时，请使用此选项。

* * *

**关于时间戳的说明：**基本属性集包括三个时间戳：`creationTime`、`lastModifiedTime`和`lastAccessTime`。 在特定实现中，这些时间戳中的任何一个可能不受支持，如果不支持，则相应的访问器方法返回一个特定于实现的值。 当支持时，时间戳作为[`FileTime`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/FileTime.html)对象返回。

* * *

以下代码片段读取并打印给定文件的基本文件属性，并使用[`BasicFileAttributes`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/BasicFileAttributes.html)类中的方法。

```java
Path file = ...;
BasicFileAttributes attr = Files.readAttributes(file, BasicFileAttributes.class);

System.out.println("creationTime: " + attr.creationTime());
System.out.println("lastAccessTime: " + attr.lastAccessTime());
System.out.println("lastModifiedTime: " + attr.lastModifiedTime());

System.out.println("isDirectory: " + attr.isDirectory());
System.out.println("isOther: " + attr.isOther());
System.out.println("isRegularFile: " + attr.isRegularFile());
System.out.println("isSymbolicLink: " + attr.isSymbolicLink());
System.out.println("size: " + attr.size());

```

除了本示例中显示的访问器方法之外，还有一个`fileKey`方法，返回一个唯一标识文件的对象，如果没有文件键可用，则返回`null`。

## 设置时间戳

以下代码片段设置最后修改时间（以毫秒为单位）：

```java
Path file = ...;
BasicFileAttributes attr =
    Files.readAttributes(file, BasicFileAttributes.class);
long currentTime = System.currentTimeMillis();
FileTime ft = FileTime.fromMillis(currentTime);
Files.setLastModifiedTime(file, ft);
}

```

## DOS 文件属性

DOS 文件属性也受支持于除 DOS 外的其他文件系统，如 Samba。 以下代码片段使用[`DosFileAttributes`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/DosFileAttributes.html)类的方法。

```java
Path file = ...;
try {
    DosFileAttributes attr =
        Files.readAttributes(file, DosFileAttributes.class);
    System.out.println("isReadOnly is " + attr.isReadOnly());
    System.out.println("isHidden is " + attr.isHidden());
    System.out.println("isArchive is " + attr.isArchive());
    System.out.println("isSystem is " + attr.isSystem());
} catch (UnsupportedOperationException x) {
    System.err.println("DOS file" +
        " attributes not supported:" + x);
}

```

但是，您可以使用[`setAttribute(Path, String, Object, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#setAttribute-java.nio.file.Path-java.lang.String-java.lang.Object-java.nio.file.LinkOption...-)方法设置 DOS 属性，如下所示：

```java
Path file = ...;
Files.setAttribute(file, "dos:hidden", true);

```

## POSIX 文件权限

*POSIX*是 UNIX 可移植操作系统接口的缩写，是一组 IEEE 和 ISO 标准，旨在确保不同 UNIX 变种之间的互操作性。 如果程序符合这些 POSIX 标准，它应该很容易移植到其他符合 POSIX 标准的操作系统。

除了文件所有者和组所有者，POSIX 还支持九种文件权限：文件所有者、同一组成员和“其他所有人”的读取、写入和执行权限。

以下代码片段读取给定文件的 POSIX 文件属性，并将其打印到标准输出。 该代码使用[`PosixFileAttributes`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/PosixFileAttributes.html)类中的方法。

```java
Path file = ...;
PosixFileAttributes attr =
    Files.readAttributes(file, PosixFileAttributes.class);
System.out.format("%s %s %s%n",
    attr.owner().getName(),
    attr.group().getName(),
    PosixFilePermissions.toString(attr.permissions()));

```

[`PosixFilePermissions`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/PosixFilePermissions.html)辅助类提供了几个有用的方法，如下所示：

+   `toString`方法，在前面的代码片段中使用，将文件权限转换为字符串（例如，`rw-r--r--`）。

+   `fromString`方法接受表示文件权限的字符串并构造一个文件权限的`Set`。

+   `asFileAttribute`方法接受一个文件权限的`Set`并构造一个可传递给`Path.createFile`或`Path.createDirectory`方法的文件属性。

以下代码片段从一个文件中读取属性并创建一个新文件，将原始文件的属性分配给新文件：

```java
Path sourceFile = ...;
Path newFile = ...;
PosixFileAttributes attrs =
    Files.readAttributes(sourceFile, PosixFileAttributes.class);
FileAttribute<Set<PosixFilePermission>> attr =
    PosixFilePermissions.asFileAttribute(attrs.permissions());
Files.createFile(file, attr);

```

`asFileAttribute`方法将权限包装为`FileAttribute`。然后代码尝试使用这些权限创建一个新文件。请注意，`umask`也适用，因此新文件可能比请求的权限更安全。

要将文件的权限设置为表示为硬编码字符串的值，可以使用以下代码：

```java
Path file = ...;
Set<PosixFilePermission> perms =
    PosixFilePermissions.fromString("rw-------");
FileAttribute<Set<PosixFilePermission>> attr =
    PosixFilePermissions.asFileAttribute(perms);
Files.setPosixFilePermissions(file, perms);

```

``Chmod``示例递归更改文件权限，类似于`chmod`实用程序。

## 设置文件或组所有者

要将名称翻译为可存储为文件所有者或组所有者的对象，您可以使用[`UserPrincipalLookupService`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/attribute/UserPrincipalLookupService.html)服务。此服务查找一个字符串作为名称或组名称，并返回表示该字符串的`UserPrincipal`对象。您可以通过使用[`FileSystem.getUserPrincipalLookupService`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getUserPrincipalLookupService--)方法获取默认文件系统的用户主体查找服务。

以下代码片段显示了如何使用`setOwner`方法设置文件所有者：

```java
Path file = ...;
UserPrincipal owner = file.GetFileSystem().getUserPrincipalLookupService()
        .lookupPrincipalByName("sally");
Files.setOwner(file, owner);

```

`Files`类中没有专门用于设置组所有者的方法。但是，直接通过 POSIX 文件属性视图安全地执行此操作的方法如下：

```java
Path file = ...;
GroupPrincipal group =
    file.getFileSystem().getUserPrincipalLookupService()
        .lookupPrincipalByGroupName("green");
Files.getFileAttributeView(file, PosixFileAttributeView.class)
     .setGroup(group);

```

## 用户定义的文件属性

如果您的文件系统实现支持的文件属性不足以满足您的需求，您可以使用`UserDefinedAttributeView`来创建和跟踪自己的文件属性。

一些实现将这个概念映射到诸如 NTFS 替代数据流和文件系统（如 ext3 和 ZFS）上的扩展属性等功能。大多数实现对值的大小施加限制，例如，ext3 将大小限制为 4 千字节。

文件的 MIME 类型可以使用此代码片段存储为用户定义的属性：

```java
Path file = ...;
UserDefinedFileAttributeView view = Files
    .getFileAttributeView(file, UserDefinedFileAttributeView.class);
view.write("user.mimetype",
           Charset.defaultCharset().encode("text/html");

```

要读取 MIME 类型属性，您可以使用以下代码片段：

```java
Path file = ...;
UserDefinedFileAttributeView view = Files
.getFileAttributeView(file,UserDefinedFileAttributeView.class);
String name = "user.mimetype";
ByteBuffer buf = ByteBuffer.allocate(view.size(name));
view.read(name, buf);
buf.flip();
String value = Charset.defaultCharset().decode(buf).toString();

```

``Xdd``示例显示了如何获取、设置和删除用户定义的属性。

* * *

**注意：**在 Linux 中，您可能需要启用扩展属性才能使用户定义的属性起作用。如果尝试访问用户定义的属性视图时收到`UnsupportedOperationException`，则需要重新挂载文件系统。以下命令重新挂载具有 ext3 文件系统扩展属性的根分区。如果此命令对您的 Linux 版本不起作用，请参考文档。

```java
$ sudo mount -o remount,user_xattr /

```

如果要使更改永久生效，请向`/etc/fstab`添加条目。

* * *

## 文件存储属性

您可以使用[`FileStore`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileStore.html)类来了解文件存储的信息，例如可用空间有多少。[`getFileStore(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#getFileStore-java.nio.file.Path-)方法获取指定文件的文件存储。

以下代码片段打印了特定文件所在文件存储的空间使用情况：

```java
Path file = ...;
FileStore store = Files.getFileStore(file);

long total = store.getTotalSpace() / 1024;
long used = (store.getTotalSpace() -
             store.getUnallocatedSpace()) / 1024;
long avail = store.getUsableSpace() / 1024;

```

`DiskUsage`示例使用此 API 打印默认文件系统中所有存储的磁盘空间信息。此示例使用`FileSystem`类中的[`getFileStores`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getFileStores--)方法来获取文件系统的所有文件存储。
