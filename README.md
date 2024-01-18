<div align="center">
<p>
 <img width="100px" src="https://raw.githubusercontent.com/NekoSilverFox/NekoSilverfox/403ab045b7d9adeaaf8186c451af7243f5d8f46d/icons/silverfox.svg" align="center" alt="NekoSilverfox" />
 <p align="center"><b><font size=6>Daily-Cheese</font></b></p>
 <p align="center"><b>每日芝士</b></p>
</p>



[![License](https://img.shields.io/badge/license-Apache%202.0-brightgreen)](LICENSE)



<div align="left">
<!-- 顶部至此截止 -->


[toc]



# 🧀基础🧀

## 上下文信息

在计算机科学和软件工程中，上下文信息（**Context** Information）**通常指的是程序执行的特定环境、状态和相关信息**。具体来说，上下文信息可能包括：

1. **环境信息：** 执行程序或操作的环境条件，例如操作系统、硬件架构、网络连接等。
2. **状态信息：** 程序或系统的当前状态，包括变量值、数据结构状态、内存内容等。
3. **配置信息：** 使用的配置选项和设置，例如文件路径、用户权限、语言设置等。
4. **调用栈信息：** 当前程序的调用栈，包括函数调用的层次和参数。
5. **时间信息：** 执行的时间戳或计时器信息，用于性能分析或定时任务。
6. **用户信息：** 对于与用户交互的应用程序，上下文信息可能包括用户的输入、偏好设置、登录状态等。

在特定的上下文中，上下文信息可以提供程序执行所需的一切信息，以便正确执行任务或操作。对于某些库或框架，上下文对象（如FFmpeg 中的`AVFormatContext`）就是用来存储和传递这些上下文信息的数据结构。

## 回调



# 🧀多线程🧀

## 进程 & 线程

**基本概念:**

- **节点**
    - ﻿服务器，等同于一台台式或者笔记本电脑。许多节点组成集群甚至是超算系统
    - ﻿节点的核数 = 该节点在不超线程下支持运行的最多线程数量
- **进程**
    - 程序运行的实例对象，进程拥有独立的堆栈以及数据，数据不能共享。一般开启的一个应用程序就是一个进程。
    - 进程可以使用MPI进行跨节点通信
- **线程**
    - 是进程中的实际运作单位，被包含在进程之中。进程可以调用多个线程来处理任务，但线程不能开启进程
    - 线程内可以有独立的内存及数据，也可以线程间共享数据
    - **线程一般用于节点内井行，一般不用做跨节点并行**
- 节点内 $进程数 * 线程数<=节点核数$
    - 假如节点有24核，运行4个进程，每个进程最多开6个线程。超线程会导致程序运行很慢很慢



并行计算又叫“**超级计算**”，因为大部分时候他是依赖于超级计算机。对于超级计算机等来说，基本并行编程概念的**颗粒度由小到大**：

1. **指令集并行** - CPU 流水线
2. **共享存储式并行** - OpenMP、OpenCL、OpenAcc
3. **分布式并行** - MPI（*Message Passing Interface*，消息传递接口）





## 互斥 

进程互斥（Mutex） - 两个或两个以上的进程，不能同时进入关于同一组共享变量的临界区域，否则可能发生与时间有关的错误，这种现象被称作进程互斥· 也就是说，一个进程正在访问[临界资源](https://baike.baidu.com/item/临界资源/1880269?fromModule=lemma_inlink)，另一个要访问该资源的进程必须等待。

在[多道程序](https://baike.baidu.com/item/多道程序/8192392?fromModule=lemma_inlink)环境下，存在着临界资源，它是指[多进程](https://baike.baidu.com/item/多进程/9796976?fromModule=lemma_inlink)存在时必须互斥访问的资源。也就是某一时刻不允许多个进程同时访问，只能单个进程的访问。我们把这些程序的片段称作[临界区](https://baike.baidu.com/item/临界区/8942134?fromModule=lemma_inlink)或临界段，它存在的目的是有效的防止[竞争条件](https://baike.baidu.com/item/竞争条件/10354815?fromModule=lemma_inlink)又能保证最大化使用共享数据。而这些并发进程必须有好的解决方案，才能防止出现以下情况：多个进程同时处于临界区，临界区外的进程阻塞其他的进程，有些进程在临界区外无休止的等待。除此以外，这些方案还不能对CPU的速度和数目做出任何的假设。只有满足了这些条件，才是一个好的解决方案

**实现进程互斥：**为实现进程互斥，可以利用软件的方法，也可以在系统中设置专门的同步机制来协调多个进程，但是所有的同步机制应该遵循四大准则：

1. **空闲让进** 当[临界资源](https://baike.baidu.com/item/临界资源/0?fromModule=lemma_inlink)处于空闲状态，允许一个请求进入临界区的进程立即进入临界区，从而有效的利用资源。

2. **忙则等待** 已经有进程进入临界区时，意味着相应的临界资源正在被访问，所以其他准备进入临界区的进程必须等待，来保证[多进程](https://baike.baidu.com/item/多进程/0?fromModule=lemma_inlink)互斥。

3. **有限等待** 对要求访问临界资源的进程，应该保证该进程能在有效的时间内进入临界区，防止死等状态。

4. **让权等待** 当进程不能进入临界区，应该立即释放[处理机](https://baike.baidu.com/item/处理机/0?fromModule=lemma_inlink)，防止进程忙等待。

早期解决进程互斥问题有软件的方法和硬件的方法，如：严格轮换法，Peterson的解决方案，[TSL](https://baike.baidu.com/item/TSL/6695760?fromModule=lemma_inlink)指令，[Swap](https://baike.baidu.com/item/Swap/2666186?fromModule=lemma_inlink)指令都可以实现进程的互斥，不过它们都有一定的缺陷，这里就不一一详细说明，而后来[Dijkstra](https://baike.baidu.com/item/Dijkstra/1880870?fromModule=lemma_inlink)提出的[信号量机制](https://baike.baidu.com/item/信号量机制/0?fromModule=lemma_inlink)则更好的解决了互斥问题。

解决进程互斥还有[管程](https://baike.baidu.com/item/管程/0?fromModule=lemma_inlink)，进程消息通信等方式。



## 互斥锁 & 临界区

**互斥锁**（英语：Mutual exclusion，缩写 Mutex）是一种用于[多线程](https://zh.wikipedia.org/wiki/多线程)[编程](https://zh.wikipedia.org/wiki/编程)中，防止两条[线程](https://zh.wikipedia.org/wiki/线程)同时对同一公共资源（比如[全局变量](https://zh.wikipedia.org/wiki/全域變數)）进行读写的机制。该目的通过将代码切片成一个一个的[临界区域](https://zh.wikipedia.org/wiki/临界区域)（critical section）达成。

**临界区**域指的是一块对公共资源进行访问的代码，并非一种机制或是算法。一个程序、进程、线程可以拥有多个临界区域，但是并不一定会应用互斥锁。

需要此机制的资源的例子有：[旗标](https://zh.wikipedia.org/wiki/旗標)、[队列](https://zh.wikipedia.org/wiki/队列)、[计数器](https://zh.wikipedia.org/wiki/计数器)、[中断](https://zh.wikipedia.org/wiki/中断)处理程序等用于在多条并行运行的代码间传递数据、同步状态等的资源。维护这些资源的同步、一致和完整是很困难的，因为一条线程可能在任何一个时刻被暂停（休眠）或者恢复（唤醒）。

例如：一段代码（甲）正在分步修改一块数据。这时，另一条线程（乙）由于一些原因被唤醒。如果乙此时去读取甲正在修改的数据，而甲碰巧还没有完成整个修改过程，这个时候这块数据的状态就处在极大的不确定状态中，读取到的数据当然也是有问题的。更严重的情况是乙也往这块地方写数据，这样的一来，后果将变得不可收拾。因此，多个线程间共享的数据必须被保护。达到这个目的的方法，就是确保同一时间只有一个临界区域处于运行状态，而其他的临界区域，无论是读是写，都必须被挂起并且不能获得运行机会。

**需求：**

- 不准永远耽搁一个要求进入临界区域的线程，造成[死锁](https://zh.wikipedia.org/wiki/死锁)（deadlock）或是[饥饿](https://zh.wikipedia.org/wiki/饥饿_(操作系统))（starvation）发生 。
- 若没有任何线程处于临界区域时，任何要求进入临界区域的线程必须立刻得到允许。
- 不能对线程的相对速度与处理器的数目做任何假设。
- 线程只能在临界区域内停留一有限的时间。
- 任何时间只允许一个线程在临界区域执行。
- 在临界区域停止执行的线程，不准影响其他线程执行。



