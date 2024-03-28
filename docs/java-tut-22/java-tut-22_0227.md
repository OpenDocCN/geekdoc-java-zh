# 问题和练习：并发

> 原文：[`docs.oracle.com/javase/tutorial/essential/concurrency/QandE/questions.html`](https://docs.oracle.com/javase/tutorial/essential/concurrency/QandE/questions.html)

## 问题

1.  你能将`Thread`对象传递给`Executor.execute`吗？这样的调用有意义吗？

## 练习

1.  编译并运行``BadThreads.java``：

    ```java

    public class BadThreads {

        static String message;

        private static class CorrectorThread
            extends Thread {

            public void run() {
                try {
                    sleep(1000); 
                } catch (InterruptedException e) {}
                // Key statement 1:
                message = "Mares do eat oats."; 
            }
        }

        public static void main(String args[])
            throws InterruptedException {

            (new CorrectorThread()).start();
            message = "Mares do not eat oats.";
            Thread.sleep(2000);
            // Key statement 2:
            System.out.println(message);
        }
    }

    ```

    应用程序应该打印出“Mares do eat oats.” 这是一定会发生的吗？如果不是，为什么？改变两次`Sleep`调用的参数会有帮助吗？如何确保所有对`message`的更改在主线程中可见？

1.  修改 Guarded Blocks 中的生产者-消费者示例，使用标准库类代替`Drop`类。

检查你的答案。
