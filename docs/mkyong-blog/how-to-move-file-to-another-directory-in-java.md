# 如何在 Java 中将文件移动到另一个目录

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/how-to-move-file-to-another-directory-in-java/>

本文展示了如何将文件移动到同一个文件驱动器或远程服务器中的另一个目录。

*   `Files.move`–在本地系统中移动文件。
*   `JSch`–移动文件以移除服务器(SFTP)

## 1.将文件移动到另一个目录

这个 Java 示例使用 NIO `Files.move`将文件从移动到同一个本地驱动器中的另一个目录。

FileMove.java

```java
 package com.mkyong.io.file;

import java.io.IOException;
import java.nio.file.*;

public class FileMove {

    public static void main(String[] args) {

        String fromFile = "/home/mkyong/data/db.debug.conf";
        String toFile = "/home/mkyong/data/deploy/db.conf";

        Path source = Paths.get(fromFile);
        Path target = Paths.get(toFile);

        try {

            // rename or move a file to other path
            // if target exists, throws FileAlreadyExistsException
            Files.move(source, target);

            // if target exists, replace it.
            // Files.move(source, target, StandardCopyOption.REPLACE_EXISTING);

            // multiple CopyOption
            /*CopyOption[] options = { StandardCopyOption.REPLACE_EXISTING,
                                StandardCopyOption.COPY_ATTRIBUTES,
                                LinkOption.NOFOLLOW_LINKS };

            Files.move(source, target, options);*/

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
} 
```

更多 Java [文件移动](/web/20220607032855/https://mkyong.com/java/how-to-rename-file-in-java/)的例子。

## 2.将文件移动到远程服务器目录

这个 Java 示例使用 [JSch](http://web.archive.org/web/20220607032855/http://www.jcraft.com/jsch/) 库将文件从本地系统移动到远程服务器的另一个目录，使用 [SFTP](http://web.archive.org/web/20220607032855/https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) 。

*P.S .假设远程服务器启用了使用密码的 SSH 登录(默认端口 22)。*

pom.xml

```java
 <dependency>
      <groupId>com.jcraft</groupId>
      <artifactId>jsch</artifactId>
      <version>0.1.55</version>
  </dependency> 
```

SFTPFileTransfer.java

```java
 package com.mkyong.io.howto;

import com.jcraft.jsch.*;

public class SFTPFileTransfer {

    private static final String REMOTE_HOST = "1.2.3.4";
    private static final String USERNAME = "";
    private static final String PASSWORD = "";
    private static final int REMOTE_PORT = 22;
    private static final int SESSION_TIMEOUT = 10000;
    private static final int CHANNEL_TIMEOUT = 5000;

    public static void main(String[] args) {

        // local
        String localFile = "/home/mkyong/hello.sh";

        // remote server
        String remoteFile = "/home/mkyong/test.sh";

        Session jschSession = null;

        try {

            JSch jsch = new JSch();
            jsch.setKnownHosts("/home/mkyong/.ssh/known_hosts");
            jschSession = jsch.getSession(USERNAME, REMOTE_HOST, REMOTE_PORT);

            // authenticate using private key
            // jsch.addIdentity("/home/mkyong/.ssh/id_rsa");

            // authenticate using password
            jschSession.setPassword(PASSWORD);

            // 10 seconds session timeout
            jschSession.connect(SESSION_TIMEOUT);

            Channel sftp = jschSession.openChannel("sftp");

            // 5 seconds timeout
            sftp.connect(CHANNEL_TIMEOUT);

            ChannelSftp channelSftp = (ChannelSftp) sftp;

            // transfer file from local to remote server
            channelSftp.put(localFile, remoteFile);

            // download file from remote server to local
            // channelSftp.get(remoteFile, localFile);

            channelSftp.exit();

        } catch (JSchException | SftpException e) {

            e.printStackTrace();

        } finally {
            if (jschSession != null) {
                jschSession.disconnect();
            }
        }

    }

} 
```

请访问此[文件传输使用 Java 中的 SFTP(JSch)](/web/20220607032855/https://mkyong.com/java/file-transfer-using-sftp-in-java-jsch/)。

**注意**NIO`Files.move`不能将文件从本地系统移动到远程服务器目录。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220607032855/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [文件 JavaDoc](http://web.archive.org/web/20220607032855/https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)
*   [移动文件或目录](http://web.archive.org/web/20220607032855/https://docs.oracle.com/javase/tutorial/essential/io/move.html)
*   [JSch](http://web.archive.org/web/20220607032855/http://www.jcraft.com/jsch/)
*   [Wikipedia SFTP](http://web.archive.org/web/20220607032855/https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)
*   [如何在 Java 中重命名或移动文件](/web/20220607032855/https://mkyong.com/java/how-to-rename-file-in-java/)
*   [在 Java 中移动文件](/web/20220607032855/https://mkyong.com/java/how-to-rename-file-in-java/)
*   [使用 Java (JSch)中的 SFTP 进行文件传输](/web/20220607032855/https://mkyong.com/java/file-transfer-using-sftp-in-java-jsch/)

<input type="hidden" id="mkyong-current-postId" value="5470">