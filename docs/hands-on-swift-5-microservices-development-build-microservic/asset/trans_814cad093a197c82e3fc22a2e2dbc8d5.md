
# 扁平化嵌套可选类型

这解决了一个问题：你试图从一个可能为 null 的对象中获取函数的返回值。例如，我们有一个 `Product` 类，它有一个返回 `Int` 实例的 `getPrice()` 函数：

```swift
class Product {
    func getPrice() throws -> Int {
        return 10
    }
}
var product: Product? = nil
var price = try? product?.getPrice()
```

在 Swift 4.2 中，`price` 会是一个“可选的可选 Int”（`Int??`）；然而，在 Swift 5 中，我们可以将最后一行改为这样：

```swift
var price = try? product?.getPrice()
```

现在我们只有一层可选类型，而不是两层。虽然这个示例很基础，但它是一个强大的功能，因为在服务器应用程序中，你可能会将许多可选函数链接在一起。

我们现在已经看到了 Swift 5 带来的新特性；其中许多对后端开发非常有用。在下一章中，我们将介绍 Vapor，这是最流行的服务端 Swift 框架。

# 总结

在本章中，你回顾了 Swift 的历史，以及它为何在其他语言中脱颖而出。然后你了解了为什么 Swift 从一开始就倾向于服务器开发。在将 Swift 的性能与其他语言和框架进行比较后，你了解了服务端 Swift 以及目前支持的内容。Docker 和 SwiftNIO 现在应该是你能够解释的术语了。最后，你探索了 Swift 5 的特性，并看到了这些变化在实践中可能是什么样子的示例。

请阅读*问题*分验证你对服务端 Swift 的理解。

在下一章中，你将探索 Vapor 框架，并准备好使用 Vapor 和 Swift 创建 Web 应用程序。

# 问题

为了检验你是否理解了服务端 Swift 的要点，请尝试回答以下问题：

1.  为什么 Swift 作为原生语言会有帮助？
2.  目前服务端 Swift 应用程序的首选操作系统是什么？
3.  什么是 SwiftNIO？
4.  Docker 如何提供帮助？
5.  说出 Swift 5 新增的三个优势。

# 开始使用 Vapor 框架

在了解了微服务和 Swift 之后，现在是时候专注于你将要使用的框架了。选择合适的框架是我们技术栈的重要组成部分。Swift 开源后，出现了几个可靠的 Web 开发框架选择。在本章中，你将探索 Vapor 框架，以及为什么本书选择它而不是其他框架。

首先，我们将了解 Vapor 框架及其脱颖而出的原因。我们将探索 Vapor 提供的功能，并查看一些代码示例。之后，我们将学习其他框架与 Vapor 的对比。

本章结构如下：

*   Vapor 框架
*   将 Vapor 与 Kitura、Perfect 和 Smoke 进行比较
*   安装 Vapor
*   Vapor 实战

让我们开始吧！

## Vapor 框架

Vapor 是目前最流行、最受喜爱的 Swift 框架。它在 GitHub 上拥有超过 18,000 颗星，并拥有一个正在构建该框架的活跃社区。在本节中，我们将深入了解该框架的设计和运作方式。

让我们看看以下内容：

*   Vapor 的历史：一切从何开始？
*   Hello World：一个简短易懂的 Vapor 应用程序
*   总体结构：了解 Vapor 的内部工作原理
*   SwiftNIO – Future 和 Promise：SwiftNIO 如何与 Vapor 集成？
*   控制器：逻辑构建块
*   数据库和模型：存储数据
*   视图：提供 HTML

### Vapor 的历史

Vapor 由 Tanner Nelson 于 2016 年发明，就在 Swift 开源后不久。初始版本依赖一些外部 C 库来处理套接字（响应 HTTP 请求），后来被重写为 Vapor 2，后者仅为 MySQL 扩展使用一个 C 库。Tanner 参考 Laravel（PHP）框架松散地编写了 Vapor，因为他从结构角度喜欢 Laravel。

Vapor 2 经历了一个强劲的增长期，期间一些企业赞助商加入支持该框架。与脚本框架类似，Vapor 2 在处理连接时采用同步方法。这成为用户的痛点，因为它可能导致应用冻结或出现其他网络问题。发生这种原因是应用没有明智地利用 CPU 时间，且可能存在竞争条件。因此，Vapor 3 从一开始就被设计为异步的。它甚至在 SwiftNIO 之前就拥有自己的异步层。2018 年初 Apple 发布 SwiftNIO 后，Vapor 在一周内就将其纳入。Vapor 3 于 2018 年 5 月正式发布。

Vapor 3 给框架带来了许多变化——主要变化是几乎每个事务现在都由 SwiftNIO 异步处理。经过几个月的错误修复，Vapor 4 的最初构想开始形成。

在撰写本文时，Vapor 4 目前处于测试阶段，是 Vapor 3 的更新和改进版本。Vapor 3 的反馈和经验被汇总到一个新版本中，包含一些破坏性更改，从而产生了 Vapor 4 版本。实际上，让 Vapor 3 应用程序在 Vapor 4 下运行并不需要太多工作。

### Hello World

Vapor 在结构和内部设置方面是有主见的。有主见的框架为你的结构提供了大量指导，例如文件格式、文件夹层次结构和命名约定。

然而，官方的“Hello World”只需七行代码：

```swift
import Vapor
let app = try Application()

app.get("hello") { req in
    return "Hello, world."
}

try app.run()
```

如你所见，Vapor 应用程序作为服务器运行只需要一个元素：应用程序对象。应用程序充当路由器，管理所有 URL 访问点，并将它们转发给控制器函数或类。

Vapor 是一个有主见的框架，这意味着它鼓励遵循某些标准来构建文件结构和文件内容。

### 总体结构

Vapor 为其应用推荐了一种结构，尽管这不是强制的。你可以在 <https://docs.vapor.codes/4.0/folder-structure/> 阅读更多关于结构的详细信息。

简化的 Vapor 应用程序结构如下：
