

如果需要一个迭代器，它始终反映当前哈希映射状态，同时被多个线程更新，那么就需要付出性能代价，而 `ConcurrentHashMap` 并不是正确的方法。这是优先考虑性能而非一致性的一个例子——这是一个经典的设计权衡。

## 总结与进一步阅读

我将在本书的其余部分贯穿本章介绍的主要概念。线程本质上是我们在构建可扩展分布式系统时所使用的數據处理和数据库平台的组成部分。在许多情况下，你可能不会显式地编写多线程代码。然而，你编写的代码将在多线程环境中被调用，这意味着你需要意识到线程安全。许多平台还通过配置参数公开其并发性，这意味着为了调整系统的性能，你需要理解更改各种线程和线程池设置的影响。基本上，在可扩展分布式系统的世界里，无法逃避并发。

最后，值得一提的是，虽然并发编程原语在不同编程语言中有所不同，但基础问题并没有改变，需要精心设计多线程代码以避免竞态条件和死锁。无论你使用的是 C/C++ 中的 `pthreads`⁸ 库，还是经典的受 Communicating Sequential Processes⁹ (CSP)启发的 Go 并发模型¹⁰，你需要避免的问题都是一样的。你从本章获得的知识无论你使用什么语言都将对你大有裨益。

本章仅触及了并发的一般性以及 Java 对其支持的表面。继续学习并发基本概念的最佳书籍是 Brian Goetz 等人撰写的经典著作 *Java Concurrency in Practice* (JCiP)。如果你理解了 JCiP 中的所有内容，你就能编写出相当出色的并发代码。

不过，自 Java 5 以来，Java 的并发支持已经有了很大的发展。在 Java 12 的世界里（或你在阅读本文时当前使用的版本），有诸如 `CompletableFutures`、lambda 表达式和并行流等新特性。Java 8.0 中引入的函数式编程风格使得无需直接创建和管理线程即可创建并发解决方案变得容易。关于 Java 8.0 特性的一个良好知识来源是 Javier Fernández González 的 *Mastering Concurrency Programming with Java 8*。

其他优秀的资料包括：

- Doug Lea, *Concurrent Programming in Java: Design Principles and Patterns*, 2nd Edition
- Raoul-Gabriel Urma, Mario Fusco, and Alan Mycroft, *Java 8 in Action: Lambdas, Streams, and functional-style programming*, Manning Publications, 1st Edition, 2014.

1 http://www.computinghistory.org.uk/det/6192/Introduction-of-Intel-386/

2 http://www.golang-book.com/books/intro/10

3 https://erlang.org/doc/getting_started/conc_prog.html

4 https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/

5 正确处理这些问题的方法，即屏障同步，将在本章后面介绍。

6 https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/package-summary.html

7 除 Vector 和 HashTable 外，它们是遗留类，线程安全且缓慢！

8 https://en.wikipedia.org/wiki/POSIX_Threads#:~:text=POSIX%20Threads%2C%20usually%20referred%20to,work%20that%20overlap%20in%20time.

9 https://en.wikipedia.org/wiki/Communicating_sequential_processes

10 https://www.golang-book.com/books/intro/10

## Part II. 设计可扩展服务

## Chapter 5. 应用服务

### A NOTE FOR EARLY RELEASE READERS

使用 Early Release 电子书，你可以在这些技术正式发布之前很久就获得书籍的早期形式——作者在撰写时的原始未编辑内容。

这将是最终书籍的第 5 章。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系编辑 vwilson@oreilly.com。

任何系统的核心都在于实现应用需求的独特业务逻辑。在分布式系统中，这通过应用程序编程接口 (API) 向客户端公开，并在为高效支持并发远程调用而设计的运行时环境中执行。API 及其实现构成了应用程序支持的服务的根本要素。

在本章中，我将重点关注实现应用程序服务层可扩展性的相关问题。我将解释 API 和服务设计，并描述提供服务执行环境的应用程序服务器的显著特征。我还将阐述我在第 2 章中简要介绍的诸如水平扩展、负载均衡和状态管理等主题。

## Service Design

在最简单的情况下，一个应用程序包含一个面向互联网的服务，该服务将数据持久化到本地数据存储，如图 5-1 所示。客户端

通过其发布的 API 与服务交互，该 API 可通过互联网访问。

让我们更详细地了解 API 和服务实现。

![](img/936a72197720a5158b16541313af61c9_125_0.png)

Figure 5-1. A Simple Service

## Application Programming Interface (API)

API 在客户端和服务器之间定义了一个契约。API 指定了可能发生的请求类型、随请求一起提供的数据以及将获得的结果。API 有许多不同的变体，正如我在第 3 章 RPC/RMI 讨论中解释的那样。虽然现代应用程序中仍然存在一些 API 多样性，但主要风格依赖于 HTTP API。这些通常被归类为 RESTful，尽管并不特别准确。

REST 实际上是一种架构风格，由 Roy Fielding 在他的博士论文¹中定义。关于 RESTful API 以及 Web 技术可以被利用的各种程度的绝佳知识来源是 Jim Webber 等人撰写的 REST in Practice (O’Reilly)。在这里，我将简要介绍 HTTP CRUD API 模式。该模式并未完全实现 REST 的原则，但在当今的互联网系统中被广泛采用。它

利用了四个代码 HTTP 动词，即 POST、GET、PUT 和 DELETE。

CRUD 代表 *Create, Read, Update, Delete*。CRUD API 指定客户端如何在特定业务上下文中执行这些操作。例如，用户可以 *create* 一个个人资料 (POST)、*read* 目录项 (GET)、*update* 他们的购物车 (PUT) 并 *delete* 他们订单中的商品 (DELETE)。

一个使用这四个核心 HTTP 动词的示例 HTTP CRUD API，用于在第 2 章中简要介绍的示例滑雪度假村系统，如表 5-1 所示。在此示例中，参数值作为请求地址的一部分传递，并由 {} 标记标识。

### Table 5-1.
HTTP CRUD Verbs

| Verb | Uniform Resource Identifier Example | Purpose |
|------|--------------------------------------|---------|
| POST | /skico.com/skiers/ | 创建新的滑雪者个人资料，滑雪者详细信息在 JSON 请求负载中提供。新的滑雪者个人资料在 JSON 响应中返回 |
| GET | /skico.com/skiers/{skierID} | 获取滑雪者的个人资料信息，以 JSON 响应负载返回 |
| PUT | /skico.com/skiers/{skierID} | 更新滑雪者个人资料 |
| DELETE | /skico.com/skiers/{skierID} | 删除滑雪者的个人资料，因为他们没有续订通行证！ |

附加参数值可以分别在 HTTP 请求和响应体中传递和返回。例如，对以下内容的成功请求：

```
GET /skico.com/skiers/12345
```

将返回 HTTP 200 响应代码和以下以 JSON 格式化的结果：

```
{
  "username": "Ian123",
  "email":"i.gorton@somewhere.com"
  "city": "Seattle"
}
```

要更改滑雪者的城市，客户端可以向同一 URI 发出以下 PUT 请求，并附带表示更新后滑雪者个人资料的请求体。

