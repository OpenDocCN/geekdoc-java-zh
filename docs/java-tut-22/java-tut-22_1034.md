# 课程：理解套接字直接协议

> 原文：[`docs.oracle.com/javase/tutorial/sdp/sockets/index.html`](https://docs.oracle.com/javase/tutorial/sdp/sockets/index.html)

对于高性能计算环境，快速高效地在网络中传输数据是必需的。这样的网络通常被描述为需要高吞吐量和低延迟。*高吞吐量*指的是在长时间内可以提供大量处理能力的环境。*低延迟*指的是处理输入并提供输出之间的最小延迟，就像您在实时应用程序中所期望的那样。

在这些环境中，使用套接字流进行传输数据可能会导致瓶颈。InfiniBand（IB）由[InfiniBand 贸易协会](http://www.infinibandta.org/)于 1999 年推出，旨在满足高性能计算的需求。IB 的最重要特性之一是远程直接内存访问（RDMA）。RDMA 使得可以直接从一台计算机的内存移动数据到另一台计算机，绕过两台计算机的操作系统，从而实现显著的性能提升。

套接字直接协议（SDP）是一种网络协议，旨在支持在 InfiniBand 结构上的流连接。SDP 支持是在 Java 平台标准版（"Java SE 平台"）的 JDK 7 版本中引入的，用于在 Solaris 操作系统（"Solaris OS"）和 Linux 操作系统上部署的应用程序。Solaris OS 自 Solaris 10 5/08 版本以来一直支持 SDP 和 InfiniBand。在 Linux 上，InfiniBand 软件包称为 OFED（OpenFabrics 企业发行版）。JDK 7 版本支持 OFED 的 1.4.2 和 1.5 版本。
