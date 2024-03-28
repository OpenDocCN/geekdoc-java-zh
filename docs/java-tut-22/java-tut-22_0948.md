# 自定义 CookieManager

> 原文：[`docs.oracle.com/javase/tutorial/networking/cookies/custom.html`](https://docs.oracle.com/javase/tutorial/networking/cookies/custom.html)

`CookieManager` 类的两个方面可以定制，即 `CookiePolicy` 和 `CookieStore`。

## CookiePolicy

为方便起见，`CookiePolicy` 定义了以下预定义的接受 cookie 的策略：

+   `CookiePolicy.ACCEPT_ORIGINAL_SERVER` 仅接受来自原始服务器的 cookie。

+   `CookiePolicy.ACCEPT_ALL` 接受所有 cookie。

+   `CookiePolicy.ACCEPT_NONE` 不接受任何 cookie。

+   你还可以通过实现 `CookiePolicy` 的 `shouldAccept` 方法来定义自己的 cookie 策略。然后，通过将其传递给多参数 `CookieManager` 构造函数或调用 `setCookiePolicy(cookiePolicy)` 方法来使用此 `CookiePolicy` 更改已经存在的 cookie 管理器。

    以下是一个拒绝来自黑名单域的 cookie，然后应用 `CookiePolicy.ACCEPT_ORIGINAL_SERVER` 策略的示例：

    ```java
    import java.net.*;

    public class BlacklistCookiePolicy implements CookiePolicy {
        String[] blacklist;

        public BlacklistCookiePolicy(String[] list) {
            blacklist = list;
        }

        public boolean shouldAccept(URI uri, HttpCookie cookie)  {
            String host;
            try {
                host =  InetAddress.getByName(uri.getHost()).getCanonicalHostName();
            } catch (UnknownHostException e) {
                host = uri.getHost();
            }

            for (int i = 0; i<blacklist.length; i++) {
    	    if (HttpCookie.domainMatches(blacklist[i], host)) {
                    return false;
                }
            }

            return CookiePolicy.ACCEPT_ORIGINAL_SERVER.shouldAccept(uri, cookie);
        }
    }

    ```

    当你创建一个 `BlacklistCookiePolicy` 实例时，你需要传递一个表示不希望接受来自的域的字符串数组。然后，将此 `BlacklistCookiePolicy` 实例设置为 `CookieManager` 的 cookie 策略。例如：

    ```java
    String[] list = new String[]{ ".example.com" };
    CookieManager cm = new CookieManager(null, new BlacklistCookiePolicy(list));
    CookieHandler.setDefault(cm);

    ```

    此示例代码将不接受来自以下主机的 cookie：

    ```java
    host.example.com
    domain.example.com

    ```

    然而，此示例代码将接受来自以下主机的 cookie：

    ```java
    example.com
    example.org
    myhost.example.org

    ```

    ## CookieStore

    `CookieStore` 是一个代表存储 cookie 的接口。`CookieManager` 为每个 HTTP 响应添加 cookie 到 `CookieStore`，并为每个 HTTP 请求从 `CookieStore` 检索 cookie。

    你可以实现此接口以提供自己的 `CookieStore` 并在创建时将其传递给 `CookieManager`。在创建 `CookieManager` 实例后无法设置 `CookieStore`。但是，你可以通过调用 `CookieManager.getCookieStore()` 来获取对 cookie 存储的引用。这样做很有用，因为它使你能够利用 Java SE 提供的默认内存中 `CookieStore` 实现，并补充其功能。

    例如，你可能想创建一个持久性 cookie 存储，以便保存 cookie，即使 Java 虚拟机重新启动也可以使用。你的实现方式类似于以下方式：

    1.  之前保存的任何 cookie 都将被读取。

    1.  在运行时，cookie 被存储和检索到内存中。

    1.  在退出之前，cookie 被写入持久存储。

    以下是此 cookie 存储的不完整示例。此示例展示了如何利用 Java SE 默认的内存中 cookie 存储，并如何扩展其功能。

    ```java
    import java.net.*;
    import java.util.*;

    public class PersistentCookieStore implements CookieStore, Runnable {
        CookieStore store;

        public PersistentCookieStore() {
            // get the default in memory cookie store
            store = new CookieManager().getCookieStore();

            // todo: read in cookies from persistant storage
            // and add them store

            // add a shutdown hook to write out the in memory cookies
            Runtime.getRuntime().addShutdownHook(new Thread(this)); 
        }

        public void run() {
            // todo: write cookies in store to persistent storage
        }

        public void	add(URI uri, HttpCookie cookie) {
            store.add(uri, cookie);
        }

        public List<HttpCookie> get(URI uri) {
            return store.get(uri);
        }

        public List<HttpCookie> getCookies() {
            return store.getCookies();
        }

        public List<URI> getURIs() {
            return store.getURIs();
        }

        public boolean remove(URI uri, HttpCookie cookie) {
            return store.remove(uri, cookie);
        }

        public boolean removeAll()  {
            return store.removeAll();
        }
    }

    ```
