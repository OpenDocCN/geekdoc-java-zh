# Java–在远程服务器上运行 shell 脚本

> 原文：<http://web.archive.org/web/20230101150211/https://mkyong.com/java/java-run-shell-script-on-a-remote-server/>

本文展示了如何使用 [JSch](http://web.archive.org/web/20220629160035/http://www.jcraft.com/jsch/) 库通过 SSH 在远程服务器上运行或执行 shell 脚本。

pom.xml

```java
 <dependency>
      <groupId>com.jcraft</groupId>
      <artifactId>jsch</artifactId>
      <version>0.1.55</version>
  </dependency> 
```

## 1.运行远程 Shell 脚本

这个 Java 示例使用 JSch 通过 SSH 登录一个远程服务器(使用密码)，并运行一个 shell 脚本`hello.sh`。

1.1 下面是一个远程服务器中的简单 shell 脚本，IP 地址是`1.1.1.1`。

hello.sh

```java
 #! /bin/sh
echo "hello $1\n"; 
```

分配了执行权限。

Terminal

```java
 $ chmod +x hello.sh 
```

1.2 在本地，我们可以使用下面的代码在远程服务器上运行或执行上面的 shell 脚本。

RunRemoteScript.java

```java
 package com.mkyong.io.howto;

import com.jcraft.jsch.*;

import java.io.IOException;
import java.io.InputStream;

public class RunRemoteScript {

    private static final String REMOTE_HOST = "1.1.1.1";
    private static final String USERNAME = "";
    private static final String PASSWORD = "";
    private static final int REMOTE_PORT = 22;
    private static final int SESSION_TIMEOUT = 10000;
    private static final int CHANNEL_TIMEOUT = 5000;

    public static void main(String[] args) {

        String remoteShellScript = "/root/hello.sh";

        Session jschSession = null;

        try {

            JSch jsch = new JSch();
            jsch.setKnownHosts("/home/mkyong/.ssh/known_hosts");
            jschSession = jsch.getSession(USERNAME, REMOTE_HOST, REMOTE_PORT);

            // not recommend, uses jsch.setKnownHosts
            //jschSession.setConfig("StrictHostKeyChecking", "no");

            // authenticate using password
            jschSession.setPassword(PASSWORD);

            // 10 seconds timeout session
            jschSession.connect(SESSION_TIMEOUT);

            ChannelExec channelExec = (ChannelExec) jschSession.openChannel("exec");

            // run a shell script
            channelExec.setCommand("sh " + remoteShellScript + " mkyong");

            // display errors to System.err
            channelExec.setErrStream(System.err);

            InputStream in = channelExec.getInputStream();

            // 5 seconds timeout channel
            channelExec.connect(CHANNEL_TIMEOUT);

            // read the result from remote server
            byte[] tmp = new byte[1024];
            while (true) {
                while (in.available() > 0) {
                    int i = in.read(tmp, 0, 1024);
                    if (i < 0) break;
                    System.out.print(new String(tmp, 0, i));
                }
                if (channelExec.isClosed()) {
                    if (in.available() > 0) continue;
                    System.out.println("exit-status: "
                         + channelExec.getExitStatus());
                    break;
                }
                try {
                    Thread.sleep(1000);
                } catch (Exception ee) {
                }
            }

            channelExec.disconnect();

        } catch (JSchException | IOException e) {

            e.printStackTrace();

        } finally {
            if (jschSession != null) {
                jschSession.disconnect();
            }
        }

    }
} 
```

输出

Terminal

```java
 hello mkyong

exit-status: 0 
```

**注意**
对于 JSch `UnknownHostKey`异常，请将远程服务器 ip 添加到`.ssh/known_hosts`中，读取[这个](/web/20220629160035/https://mkyong.com/java/jsch-unknownhostkey-exception/)。

## 2.运行远程命令

2.1 下面的例子与上面的例子#1 非常相似。相反，它使用私钥`id_rsa`来 SSH 登录远程服务器，确保[远程服务器正确配置了公钥](http://web.archive.org/web/20220629160035/https://www.linode.com/docs/security/authentication/use-public-key-authentication-with-ssh/)。

```java
 jsch.addIdentity("/home/mkyong/.ssh/id_rsa"); 
```

2.2 并且我们把命令改成了`ls`，剩下的代码都是一样的。

```java
 channelExec.setCommand("ls -lsah"); 
```

2.3 这个 Java 示例运行一个远程服务器命令`ls -lsah`来显示目录列表。

RunRemoteCommand.java

```java
 package com.mkyong.io.howto;

import com.jcraft.jsch.*;

import java.io.IOException;
import java.io.InputStream;

public class RunRemoteCommand {

    private static final String REMOTE_HOST = "1.1.1.1";
    private static final String USERNAME = "";
    private static final int REMOTE_PORT = 22;
    private static final int SESSION_TIMEOUT = 10000;
    private static final int CHANNEL_TIMEOUT = 5000;

    public static void main(String[] args) {

        Session jschSession = null;

        try {

            JSch jsch = new JSch();
            jsch.setKnownHosts("/home/mkyong/.ssh/known_hosts");
            jschSession = jsch.getSession(USERNAME, REMOTE_HOST, REMOTE_PORT);

            // not recommend, uses jsch.setKnownHosts
            //jschSession.setConfig("StrictHostKeyChecking", "no");

            // authenticate using private key
            jsch.addIdentity("/home/mkyong/.ssh/id_rsa");

            // 10 seconds timeout session
            jschSession.connect(SESSION_TIMEOUT);

            ChannelExec channelExec = (ChannelExec) jschSession.openChannel("exec");

            // Run a command
            channelExec.setCommand("ls -lsah");

            // display errors to System.err
            channelExec.setErrStream(System.err);

            InputStream in = channelExec.getInputStream();

            // 5 seconds timeout channel
            channelExec.connect(CHANNEL_TIMEOUT);

            // read the result from remote server
            byte[] tmp = new byte[1024];
            while (true) {
                while (in.available() > 0) {
                    int i = in.read(tmp, 0, 1024);
                    if (i < 0) break;
                    System.out.print(new String(tmp, 0, i));
                }
                if (channelExec.isClosed()) {
                    if (in.available() > 0) continue;
                    System.out.println("exit-status: "
                        + channelExec.getExitStatus());
                    break;
                }
                try {
                    Thread.sleep(1000);
                } catch (Exception ee) {
                }
            }

            channelExec.disconnect();

        } catch (JSchException | IOException e) {

            e.printStackTrace();

        } finally {
            if (jschSession != null) {
                jschSession.disconnect();
            }
        }

    }
} 
```

输出

Terminal

```java
 otal 48K
4.0K drwx------  6 root root 4.0K Aug  4 07:57 .
4.0K drwxr-xr-x 22 root root 4.0K Sep 11  2019 ..
8.0K -rw-------  1 root root 5.5K Aug  3 08:50 .bash_history
4.0K -rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
4.0K drwx------  2 root root 4.0K Dec 15  2019 .cache
4.0K drwx------  2 root root 4.0K Dec 15  2019 .docker
4.0K -rwxr-xr-x  1 root root   31 Aug  4 07:54 hello.sh
4.0K -rw-r--r--  1 root root  148 Aug 17  2015 .profile
4.0K drwx------  2 root root 4.0K Aug  3 05:32 .ssh
4.0K drwxr-xr-x  2 root root 4.0K Aug  3 04:42 test
4.0K -rw-------  1 root root 1.2K Aug  4 07:57 .viminfo
exit-status: 0 
```

2.4 现在，我们测试一个无效命令`abc`

RunRemoteCommand.java

```java
 // Run a invalid command
  channelExec.setCommand("abc"); 
```

输出

Terminal

```java
 bash: abc: command not found
exit-status: 127 
```

错误将输出到`System.err`。

## 下载源代码

$ git 克隆[https://github.com/mkyong/core-java](http://web.archive.org/web/20220629160035/https://github.com/mkyong/core-java)

$ cd java-io

## 参考

*   [Shell 脚本教程](http://web.archive.org/web/20220629160035/https://www.shellscript.sh/index.html)
*   [JSch 示例–Exec.java](http://web.archive.org/web/20220629160035/http://www.jcraft.com/jsch/examples/Exec.java.html)
*   [JSch 官方网站](http://web.archive.org/web/20220629160035/http://www.jcraft.com/jsch/)
*   [JSch–无效 privatekey 异常](/web/20220629160035/https://mkyong.com/java/jsch-invalid-privatekey-exception/)
*   [jsch–unknown STK 例外](/web/20220629160035/https://mkyong.com/java/jsch-unknownhostkey-exception/)

<input type="hidden" id="mkyong-current-postId" value="16102">