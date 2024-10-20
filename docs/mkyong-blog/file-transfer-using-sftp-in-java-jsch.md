# 使用 Java 中的 SFTP 进行文件传输(JSch)

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/file-transfer-using-sftp-in-java-jsch/>

本文展示了如何使用 Java 中的 [SSH 文件传输协议(SFTP)](http://web.archive.org/web/20220629160038/https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) 在远程服务器和本地系统之间进行文件传输。

*PS 用[JSch 0 . 1 . 55](http://web.archive.org/web/20220629160038/http://www.jcraft.com/jsch/)测试*

## 1.JSch 依赖性

pom.xml

```java
 <dependency>
      <groupId>com.jcraft</groupId>
      <artifactId>jsch</artifactId>
      <version>0.1.55</version>
  </dependency> 
```

## 2.文件传输–JSch 示例

2.1 在`JSch`中，我们可以使用`put`和`get`在服务器之间进行文件传输。

我们使用`put`将文件从本地系统传输到远程服务器。

```java
 channelSftp.put(localFile, remoteFile); 
```

我们使用`get`将文件从远程服务器下载到本地系统。

```java
 channelSftp.get(remoteFile, localFile); 
```

2.2 密码认证。

```java
 JSch jsch = new JSch();
  jsch.setKnownHosts("/home/mkyong/.ssh/known_hosts");
  jschSession = jsch.getSession(USERNAME, REMOTE_HOST, REMOTE_PORT);

  jschSession.setPassword(PASSWORD); 
```

2.3 公钥和私钥认证，阅读此[使用 SSH 的公钥认证](http://web.archive.org/web/20220629160038/https://www.linode.com/docs/security/authentication/use-public-key-authentication-with-ssh/)

*   本地私钥-`/home/mkyong/.ssh/id_rsa`
*   远程公钥-`~/.ssh/authorized_keys`

```java
 JSch jsch = new JSch();
  jsch.setKnownHosts("/home/mkyong/.ssh/known_hosts");
  jschSession = jsch.getSession(USERNAME, REMOTE_HOST, REMOTE_PORT);

  jsch.addIdentity("/home/mkyong/.ssh/id_rsa"); 
```

2.4 查看这个完整的`JSch`示例，将文件从本地系统传输到远程服务器`1.2.3.4`，使用 SSH 密码进行身份验证。

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

        String localFile = "/home/mkyong/local/random.txt";
        String remoteFile = "/home/mkyong/remote/afile.txt";

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

        System.out.println("Done");
    }

} 
```

## 3.JSch 异常

一些常见的例外。

3.1 对于 [UnknownHostKey 异常](/web/20220629160038/https://mkyong.com/java/jsch-unknownhostkey-exception/)，将远程 IP 地址添加到`known_hosts`文件中。

Terminal

```java
 $ ssh-keyscan -t rsa 1.2.3.4 >> ~/.ssh/known_hosts 
```

3.2 对于[无效的私钥](/web/20220629160038/https://mkyong.com/java/jsch-invalid-privatekey-exception/)，将私钥转换为另一种格式。

Terminal

```java
 $ ssh-keygen -p -f ~/.ssh/id_rsa -m pem 
```

3.3 对于`Auth fail`，确保提供的密码正确。

Terminal

```java
 com.jcraft.jsch.JSchException: Auth fail
	at com.jcraft.jsch.Session.connect(Session.java:519)
	at com.mkyong.io.howto.SFTPFileTransfer.main(SFTPFileTransfer.java:34) 
```

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160038/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [维基百科–SSH 文件传输协议(SFTP)](http://web.archive.org/web/20220629160038/https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)
*   [JSch–示例](http://web.archive.org/web/20220629160038/http://www.jcraft.com/jsch/examples/)
*   [jsch unnowstk 例外](/web/20220629160038/https://mkyong.com/java/jsch-unknownhostkey-exception/)
*   [JSch 无效 privatekey](/web/20220629160038/https://mkyong.com/java/jsch-invalid-privatekey-exception/)

<input type="hidden" id="mkyong-current-postId" value="16093">