# 死锁

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html)

*死锁* 描述了两个或更多线程永远被阻塞，彼此等待的情况。这里有一个例子。

阿方索和加斯顿是朋友，也是极信奉礼貌的人。一个严格的礼貌规则是，当你向朋友鞠躬时，你必须保持鞠躬的姿势，直到你的朋友有机会回礼。不幸的是，这个规则没有考虑到两个朋友可能同时向对方鞠躬的可能性。这个示例应用程序，``死锁``，模拟了这种可能性：

```java

public class Deadlock {
    static class Friend {
        private final String name;
        public Friend(String name) {
            this.name = name;
        }
        public String getName() {
            return this.name;
        }
        public synchronized void bow(Friend bower) {
            System.out.format("%s: %s"
                + "  has bowed to me!%n", 
                this.name, bower.getName());
            bower.bowBack(this);
        }
        public synchronized void bowBack(Friend bower) {
            System.out.format("%s: %s"
                + " has bowed back to me!%n",
                this.name, bower.getName());
        }
    }

    public static void main(String[] args) {
        final Friend alphonse =
            new Friend("Alphonse");
        final Friend gaston =
            new Friend("Gaston");
        new Thread(new Runnable() {
            public void run() { alphonse.bow(gaston); }
        }).start();
        new Thread(new Runnable() {
            public void run() { gaston.bow(alphonse); }
        }).start();
    }
}

```

当 `死锁` 运行时，当它们尝试调用 `bowBack` 时，两个线程都很可能被阻塞。由于每个线程都在等待另一个线程退出 `bow`，因此这两个阻塞永远不会结束。
