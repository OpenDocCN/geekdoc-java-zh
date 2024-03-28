# ExampleGame

> 原文：[`docs.oracle.com/javase/tutorial/security/userperm/game.html`](https://docs.oracle.com/javase/tutorial/security/userperm/game.html)

下面是`ExampleGame`的源代码。为简单起见，`ExampleGame`实际上并不包含玩游戏的代码。它只是检索或更新用户的最高分。

要查看用户当前的最高分值，您可以运行：

```java
java ExampleGame get

```

要为用户设置新的最高分值，您可以运行：

```java
java ExampleGame set *score* 

```

要检索用户当前的最高分，`ExampleGame`只需实例化一个`HighScore`对象并调用其`getHighScore`方法。要为用户设置新的最高分，`ExampleGame`实例化一个`HighScore`对象并调用`setHighScore`，将用户的新最高分传递给它。

这里是`ExampleGame`的源代码，`ExampleGame.java`:

```java

package com.gamedev.games;

import java.io.*;
import java.security.*;
import java.util.Hashtable;
import com.scoredev.scores.*;

public class ExampleGame
{
    public static void main(String args[])
	throws Exception 
    {
	HighScore hs = new HighScore("ExampleGame");

	if (args.length == 0)
	    usage();

	if (args[0].equals("set")) {
	    hs.setHighScore(Integer.parseInt(args[1]));
	} else if (args[0].equals("get")) {
	    System.out.println("score = "+ hs.getHighScore());
	} else {
	    usage();
	}
    }

    public static void usage()
    {
	System.out.println("ExampleGame get");
	System.out.println("ExampleGame set <score>");
	System.exit(1);
    }
}

```
